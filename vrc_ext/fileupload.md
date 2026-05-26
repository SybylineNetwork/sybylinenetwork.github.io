# File Uploads

The VRChat API requires some non-trivial file uploads to conform to a specific recipe, portrayed here as pseudocode:

```ts
const vrcapi={
    get:(url)=>fetch(url,{method:'GET'}),
    put:(url,body)=>fetch(url,{method:'PUT',headers:{'Content-Type':'application/json'},body:JSON.stringify(body)}),
    put2:(url,body,mime,md5)=>fetch(url,{method:'PUT',headers:{'Content-Type':mime,'Content-MD5':md5},body:body}),
    post:(url,body)=>fetch(url,{method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify(body)}),
    delete:(url)=>fetch(url,{method:'DELETE'})
};
const CHUNK_SIZE = 100 * 1024 * 1024;
const Librsync = loadLibrary('rsync');

function UploadFile(path filePath, string fileId, string fileNameFriendly) -> string?
{
    var extension = getExtension(filePath.name);
    var mimeType = mimeTypeFromExtension(extension);
    var creating = fileId === null || fileId.isEmpty;
    var theFile = creating
        ? vrcapi.post('/file',{
              'name':fileNameFriendly,
              'mimeType':mimeType,
              'extension':extension
          })
        : vrcapi.get(`/file/{fileId}`);
    fileId = theFile.id;
    if (fileId === null || fileId.isEmpty)
        return null;
    if (theFile.hasQueuedOperation || theFile.versions[theFile.latestVersion].isErrored)
    {
        sleep(1000);
        vrcapi.delete(`/file/{fileId}`);
        theFile = vrcapi.get(`/file/{fileId}`);
    }
    var sigPath = filePath.resolve(`{new UUID()}.sig`);
    var sigMimeType = mimeTypeFromExtension('.sig');
    var fileSize, sigSize;
    var fileMD5 = {}, sigMD5 = {};
    try
    {
        with fileIn = open(filePath, 'rb');
        with fileSigOut = open(sigPath, 'wb');
        Librsync.computeSig(fileIn) >> fileSigOut;
        
        sigSize = fileSize(sigPath);
        fileSize = fileSize(filePath);
        
        fileMD5.bytes = computeContentsMd5(filePath);
        fileMD5.b64 = toB64(fileMD5.bytes);
        
        sigMD5.bytes = computeContentsMd5(sigPath);
        sigMD5.b64 = toB64(sigMD5.bytes);
    }
    catch
    {
        return null;
    }
    var retrying = false;
    if (theFile.hasExistingOrPendingVersion)
    {
        if (fileMD5.b64 === theFile.versions[theFile.latestVersion].file.md5)
        {
            if (theFile.latestVersion.isWaiting)
                return null;
            isRetrying = true;
        }
        else if (theFile.latestVersion.isWaiting)
        {
            vrcapi.delete(`/file/{fileId}/{theFile.latestVersion}`);
            sleep(1000);
            theFile = vrcapi.get(`/file/{fileId}`);
        }
    }
    var alreadyExists = false;
    if (retrying)
    {
        var fileVersion = theFile.versions[theFile.latestVersion];
        if (fileSize === fileVersion.file.size && fileMD5.b64 === fileVersion.file.md5
            && sigSize === fileVersion.signature.size && sigMD5.b64 === fileVersion.signature.md5)
        {
            alreadyExists = true;
        }
        else
        {
            vrcapi.delete(`/file/{fileId}/{theFile.latestVersion}`);
            sleep(1000);
            theFile = vrcapi.get(`/file/{fileId}`);
        }
    }
    if (!alreadyExists)
    {
        theFile = vrcapi.post(`/file/{fileId}`, {
            'fileSizeInBytes':fileSize,
            'fileMd5':fileMD5.b64,
            'signatureSizeInBytes':sigSize,
            'signatureMd5':sigMD5.b64
        });
        fileId = theFile.id;
        if (fileId === null || fileId.isEmpty)
            return null;
        sleep(1000);
    }
    var fileVersion = theFile.versions[theFile.latestVersion];
    if (fileVersion.file.status !== 'waiting')
        return null;
    if (!UploadFileData(filePath, fileVersion.file.category, creating, 'file', fileId, theFile, mimeType, fileMD5.bytes))
        return null;
    sleep(1000);
    if (!UploadFileData(filePath, fileVersion.signature.category, creating, 'signature', fileId, theFile, sigMimeType, sigMD5.bytes))
        return null;
    sleep(1000);
    theFile = vrcapi.get(`/file/{fileId}`);
    assert theFile.versions[theFile.latestVersion].file.status === 'complete';
    assert theFile.versions[theFile.latestVersion].signature.status === 'complete';
    sleep(5000);
    theFile = vrcapi.get(`/file/{fileId}`);
    return theFile.versions[theFile.latestVersion].file.url;
};

function UploadFileData(path filePath, string type, string category, bool creating, string fileId, fileObj theFile, string mime, byte[] md5) -> bool
{
    if (category === 'simple'
        ? !UploadFileSimple(filePath, type, fileId, theFile, mime, md5)
        : !UploadFileMultipart(filePath, type, fileId, theFile, mime, md5))
    {
        if (creating)
            vrcapi.delete(`/file/{fileId}`);
        return false;
    }
    return true;
};


function UploadFileSimple(path filePath, string type, string fileId, fileObj theFile, string mimeType, byte[] md5) -> bool
{
    var start = vrcapi.put(`/file/{fileId}/{theFile.latestVersion}/{type}/start`);
    var uploadUrl = start.url;
    if (uploadUrl === null || uploadUrl.isEmpty)
        return false;
    var bytes = readAllBytes(filePath);
    try
    {
        vrcapi.put2(uploadUrl, bytes, mimeType, md5);
    }
    catch
    {
        return false;
    }
    vrcapi.put(`/file/{fileId}/{theFile.latestVersion}/{type}/finish`);
    return true;
};

function UploadFileMultipart(path filePath, string type, string fileId, fileObj theFile, string mimeType, byte[] md5) -> bool
{
    var status = try vrcapi.get(`/file/{fileId}/{theFile.latestVersion}/{type}/status`)
                 catch return false;
    var nextPartNumber = 1;
    var etags = [];
    nextPartNumber += status.nextPartNumber;
    var statusEtags = status.etags;
    etags.appendAll(statusEtags);
    with stream = open(filePath, 'rb');
    var parts = max(1, floorInt((float)stream.length / (float)CHUNK_SIZE));
    var buffer = new byte[CHUNK_SIZE * 2];
    for (var partNumber = nextPartNumber; partNumber <= parts; partNumber++)
    {
        var start = try vrcapi.put(`/file/{fileId}/{theFile.latestVersion}/{type}/start?partNumber={partNumber}`)
                    catch return false;
        var uploadUrl = start.url;
        if (uploadUrl === null || uploadUrl.isEmpty)
            return false;
        var bytesToRead = partNumber < parts ? CHUNK_SIZE : (int)(stream.length - stream.position);
        var bytesRead = 0;
        try
        {
            bytesRead = stream.read(buffer, 0, bytesToRead);
            
            var resultEtag = vrcapi.put2(uploadUrl, bytes).responseMessage.headers.etag;
            if (resultEtag !== null)
            {
                etags.append(resultEtag.tag.trim('"','\''));
            }
        }
        catch
        {
            return false;
        }
        sleep(1000);
    }
    try
    {
        vrcapi.put(`/file/{fileId}/{theFile.latestVersion}/{type}/finish`, {
            'etags':etags
        });
    }
    catch
    {
        return false;
    }
    return true;
};

```
