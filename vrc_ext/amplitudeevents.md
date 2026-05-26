# VRChat Amplitude Cache

VRChat stores Amplitude analytics data temporarily before submitting it for processing.
On Windows, the temporary file is located at `%TEMP%\VRChat\VRChat\amplitude.cache`.
The cache contains JSON data of the following format, although the specific properties in `event_properties` and `user_properties` may vary or be absent:
```json
[   // Pending submitted events are in an enclosing array

    {   // An event

        "app_version": <string>, // VRChat client version in "<year>.<major>.<minor>-<build>-<hash>-<branch>", such as "2026.1.1-1781-bb19ede9ae-Release"

        "device_id": <string>, // 20-byte hexadecimal identifier, such as "00112233445566778899aabbccddeeff00112233"

        "device_model": <string>, // Computer model (usually motherboard name), such as "Z590 UD (Gigabyte Technology Co., Ltd.)"

        "device_name": <string>, // Computer name, such as "VINYARION-PC"

        "event_id": <integer>, // Incrementing counter; resets to 0 after a new client process starts

        "event_properties": {

            ... // Data specific to a given "event_type", described in detail below

        },

        "event_type": <string>, // The type of event that occurred submitted, such as "$exposure"

        "insert_id": <integer>, // 32-bit signed integer with unknown semantics, such as 960872925

        "ip": <string>, // Always seems to be "$remote"

        "language": <string>, // Language selected, such as "English"

        "os_name": <string>, // Name of the operating system, such as "Windows 11  (10.0.22631) 64bit"

        "os_version": <string>, // Version of the operating system; on Windows, this appears always to be empty: ""

        "platform": <string>, // Unity platform running, such as "WindowsPlayer"

        "session_id": <integer>, // Unix timestamp (milliseconds) at which that the current client process was launched

        "time": <integer?>, // Unix timestamp (milliseconds) at which that this event was generated

        "user_id": <string>, // UserID of the currently authenticated player, "usr_..."

        "user_properties": {

            // System information

            "defaultFormatHDR": <string>, // Default binary HDR format, such as "R16G16B16A16_SFloat"

            "defaultFormatLDR": <string>, // Default binary LDR format, such as "R8G8B8A8_SRGB"

            "deviceId": <string>, // Same as "device_id" above

            "downloadSpeed": <string>, // Estimated connection speed (in bps)

            "graphicsDeviceName": <string>, // Name of the graphics device in use by the process, such as "NVIDIA GeForce RTX 3080"

            "graphicsDeviceVendor": <string>, // Vendor of the above device, such as "NVIDIA"

            "graphicsDeviceVersion": <string>, // Version of the graphics API supported by the above device, such as "Direct3D 11.0 [level 11.1]"

            "graphicsMemorySize": <integer>, // Graphics device VRAM (in MB) such as 12093

            "isGeForceNow": <boolean>, // Whether GeForceNow is running on the system, such as true

            "IsNvidiaGeForceNow": <boolean>, // Whether GeForceNow is running on the system, such as true (duplicate)

            "isOnMeteredConnection": <boolean>, // Whether Internet connectivity is metered, such as having limited mobile data

            "mainThreadPriority": <string>, // Priority of the main thread of the process, such as "Normal"

            "operatingSystem": <string>, // Same as "os_name" above

            "processAffinity": <integer>, // Bitmask for the process affinity, such as 0

            "processAffinityManuallySet": <boolean>, // Whether the process affinity has been set manually, such as false

            "processorFrequency": <integer>, // Base frequency of the CPU (in MHz), such as 2496

            "processorName": <string>, // Name of the CPU, such as "11th Gen Intel(R) Core(TM) i7-11700 @ 2.50GHz"

            "processPriority": <string>, // Priority of the entire process, such as "Normal"

            "systemMemorySize": <integer>, // System RAM (in MB), such as 65405

            // Build information

            "buildType": <string>, // Build type for the installed version, such as "steam"

            "buildVersion": <string>, // Same as "app_version" above

            "platform": <string>, // Same as "platform" above

            "publicbetas": <string>, // Release installed, such as "open-beta"

            "releasePlatform": <string>, // Short name for the platform, such as "Windows"

            // 3rd Person information

            "thirdPersonViewActiveRatio": <integer>,

            "thirdPersonViewActiveSeconds": <integer>,

            "thirdPersonViewInactiveSeconds": <integer>,

            "thirdPersonViewNumTimesFreeRotatePressed": <integer>,

            "thirdPersonViewNumTimesKeybindToggled": <integer>,

            "thirdPersonViewNumTimesScrollToggled": <integer>,

            "thirdPersonViewNumTimesToggled": <integer>,

            // VR information

            "numExtraVRTrackers": <integer>, // Number of extra/unused VR trackers active, such as 0

            "numOpenVRTrackers": <integer>, // Number of OpenVR trackers active, such as 0

            "numOpenXRTrackers": <integer>, // Number of OpenXR trackers active, such as 0

            "numOSCTrackers": <integer>, // Number of OSC trackers active, such as 0

            "steamVRActualTrackingSystemName": <string>, // Actual SteamVR tracking system name, such as "lighthouse"

            "steamVRTrackingSystemName": <string>, // Observed SteamVR tracking system name, such as "lighthouse"

            "vrDeviceModel": <string>, // Model of HMD in use, such as "OpenVR Headset(Index)"

            "vrDeviceRefreshRate": <integer>, // Current refresh rate of the HMD, such as 80

            "vrDeviceRenderScale": <double>, // Current render scale, such as 1

            // User state information

            "acceptedTOSVersion": <integer>, // Latest version of the Terms the user has agreed to, such as 12

            "accountType": <string>, // Account type used to authenticate, such as "steam"

            "currentAvatarAuthorId": <string>, // Id of current avatar author, such as "usr_533444de-af1a-423a-9396-f4acdf9c3125"

            "currentAvatarAuthorName": <string>, // Name of current avatar author, such as "ふかちゃん"

            "currentAvatarId": <string>, // Id of current avatar, such as "avtr_8e07afd3-4465-47b1-9dda-ed82a546c357"

            "currentAvatarName": <string>, // Name of current avatar, such as "sacabambaspis"

            "currentAvatarPerformance": <string>, // Performance of current avatar, such as "Excellent"

            "currentWorldId": <string>, // Id of current world, such as "local:error_2571820452604316", or "wrld_1a8b8684-3b19-4770-a4a7-288762f57b29"

            "currentWorldName": <string>, // Name of current world, such as "Error World", or "1's Optimized Box"

            "currentlyappliedaccessoriesids": <array>, // IDs of currently applied avatar accessories, such as [ "avp_b3211b05-c460-46e6-81f3-52030e1f0707" ]

            "currentlyappliedtotalaccessoriescount": <integer>, // Total number of currently applied avatar accessories, such as 1

            "currentlyapplieduniqueaccessoriescount": <integer>, // Total number of unique currently applied avatar accessories, such as 1

            "current_drone_skin_id": <string>, // Name of selected drone skin, such as "default_drone", or "invt_3966d26e-ef96-47f0-ba59-e533e4de144e"

            "current_drone_skin_name": <string>, // Name of selected drone skin, such as "Default Drone", or "Trace Drone"

            "current_loading_screen_id": <string>, // Name of selected loading screen, such as "random", or "invt_e5398be4-2ef9-4fcc-849c-1e9dc771664a"

            "current_loading_screen_name": <string>, // Name of selected warp effect, such as "Random", or "Classic Warp Tunnel"

            "current_portal_skin_id": <string>, // Name of selected warp effect, such as "none", or "random"

            "current_portal_skin_name": <string>, // Name of selected warp effect, such as "None", or "Random"

            "current_warp_effect_id": <string>, // Name of selected warp effect, such as "none", "random", or "invt_022f4c96-c803-46bc-87da-5ec650af4994"

            "current_warp_effect_name": <string>, // Name of selected warp effect, such as "none", "Random", or "Reference Cube Warp"

            "custom_emoji_count": <integer>, // Number of custom props, such as 15

            "custom_prop_count": <integer>, // Number of custom props, such as 0

            "developerType": <string>, // Developer type of the current user, such as "None"

            "displayName": <string>, // Display name of the user

            "emoji_group_count": <integer>, // Number of custom groups for props, such as 3

            "exclusive_emoji_count": <integer>, // Number of exclusive emoji owned, such as 5

            "exclusive_prop_count": <integer>, // Number of exclusive props owned, such as 11

            "exclusive_sticker_count": <integer>, // Number of exclusive stickers owned, such as 24

            "ips_category_weights": <array>, // IPS category weights, such as [ { "category": "announcement", "weight": 1 }, { "category": "informational", "weight": 89 }, { "category": "promotional", "weight": 10 } ]

            "ips_segment": <string>, // IPS segment, such as "vrcPlus", or "nonVrcPlus"

            "joinedGroupIds": <array>, // IDs of joined groups, such as [ "grp_a2ba31eb-da7e-4b37-9562-7c0fa1872fd4", "grp_34e02a44-ca82-458f-904b-9a4b20869c77" ]

            "joinedGroupNames": <array>, // Name of joined groups, such as [ "Are You Following ToS?", "VRChat Open Beta" ]

            "looksowned_count": <integer>, // Number of looks owned, such as 0

            "numberOfCustomEmoji": <integer>, // Current number of custom emoji uploaded, such as 15

            "numberOfFriends": <integer>, // Total number of friends, such as 552

            "numberOfGroups": <integer>, // Total number of groups, such as 87

            "numberOfStickersInGallery": <integer>, // Current number of stickers uploaded, such as 17

            "numPrintsInGallery": <integer>, // Current number of prints uploaded, such as 63

            "prop_group_count": <integer>, // Number of custom groups for props, such as 1

            "store": <string>, // Which store to use for general purchases, such as "Steam"

            "sticker_group_count": <integer>, // Number of custom groups for stickers, such as 3

            "subscriptionStore": <string>, // Which store to use for VRChat+ subscription purchases, such as "Steam"

            "subscriptionType": <string>, // Current VRChat+ subscription type, such as "vrchatplus-monthly"

            "total_inventory_count": <integer>,

            "totalaccessoriesownedcount": <integer>, // Current number of avatar accessories owned, such as 3

            "userIcons": <integer>, // Current number of user icons uploaded, such as 37

            "vrcplus_subscriber_exclusive_prop_count": <integer>, // Number of vrcplus-exclusive props owned, such as 1

            // Usage information

            "eyeLidTrackingType": <string>, // OSC eyelid tracking type, like "none", or "combined"

            "eyeLookTrackingType": <string>, // OSC eye look tracking type, like "none", or "combinedWithDistance"

            "numTimesDisabledSelfieExpression": <integer>,

            "numTimesEnabledSelfieExpression": <integer>,

            "numTimesRefusedSelfieExpressionPopup": <integer>,

            "numTimesRefusedSelfieExpressionPopupVrcPlus": <integer>,

            "numTimesSelfieExpressionPopupVrcPlusClicked": <integer>,

            // Settings information

            "avatarPerfMinRatingToDisplay": <string>, // Minimum performance level for avatars, such as "VeryPoor"

            "hudEnabled": <boolean>, // Whether the HUD is enabled, such as true

            "inputMethod": <string>, // Current input system in use, such as "Count", "SteamVR2", "Mouse"

            "inputType": <string>, // Current type system in use, such as "none", "keyboard"

            "inVRMode": <boolean>, // Whether the client is in VR mode, such as true

            "micEnabled": <boolean>, // Whether the microphone is enabled, such as false

            "nameplatesEnabled": <boolean>, // Whether nameplates are enabled, such as true

            "safety_{TrustLevel}_Can{*}": <boolean>, // Current cus

            "safetyLevel": <integer>, // Index of the selected safety level, such as 5

            "safetyLevelName": <string>, // Name of the selected safety level, such as "Custom"

            "setting_{*}": <boolean|integer|double|string>, // Settings values

            // UI information

            "uiLastMenuOpened": <string>, // Most recent menu opened, such as "QuickMenu"

            "uiMainMenu.{*}Selector.SelectedButton": <string>, // Which button was clicked, such as "Profile" for the key "uiMainMenu.MainMenuProfileSelector.SelectedButton"

            "uiMainMenuCurrentPage": <string>, // Which page is open on the main menu, such as "MainMenuWorldInformation"

            "uiMainMenuPageStack": <array>, // Stack of pages (bottom to top) of the main menu, such as [ "MainMenuSocial", "MainMenuWorldInformation", "MainMenuUserDetails" ]

            "uiQuickMenuCurrentPage": <string>, // Which page is open on the quick menu, such as "QuickMenuLaunchpad"

            "uiQuickMenuIsAttachedToHand": <boolean>, // Whether the quick menu is attached to a hand, such as true

            "uiQuickMenuIsOnRightHand": <boolean>, // Whether the hand the quick menu attaches to is the right hand, such as false

            "uiQuickMenuPageStack": <array>, // Stack of pages (bottom to top) of the quick menu, such as [ "QuickMenuLaunchpad" ]

            "uiWing{Left|Right}CurrentPage": <string>, // Which page is open on the left/right wing menu, such as "Emotes"

            "uiWing{Left|Right}IsOpen": <boolean>, // Whether the left/right wing menu is expanded, such as true

            "uiWing{Left|Right}PageStack": <array> // Stack of pages (bottom to top) of the left/right wing menu, such as [ "Root", "Emotes" ]

        }

    },

    
    ... // More events might be present

]
```

Example pseudocode outlining how one could reasonably read this file without duplicating events:
```pseudocode
process(callback)
    cache_file = file("%TEMP%\VRChat\VRChat\amplitude.cache")
    last_modified = 0
    cache_data = []
    last_event_ids = []
    last_session_id = 0
    while (true)
        sleep(1000ms)
        this_modified = cache_file.last_modified
        if (last_modified < this_modified)
            continue while
        last_modified = this_modified
        try
            cache_data = json.parse(cache_file)
        catch e
            continue while
        this_event_ids = []
        foreach (event_object in cache_data)
            this_event_ids.add(event_object.event_id)
            if (last_event_ids.contains(event_object.event_id))
                continue foreach
            if (0 == event_object.event_id and last_session_id == event_object.session_id)
                continue foreach
            last_session_id = event_object.session_id
            callback(event_object)
        last_event_ids = this_event_ids
```
It could be more prudent, perhaps, to use a method that listens for when the file changes rather than repeatedly querying it.

## Amplitude Event Types

Some of these events may be deprecated; this is simply a list of all known events.

### Common Event Properties

Most events have the following properties:
```json
{
    "groupAccessType": <string?>,
    "instanceType": <integer|string>, // exact property type seems to vary between specific event type
    "instanceTypeCombined": <string?>, // seems to mirror 'instanceType', but is only on newer events
    "locationId": <string>, // format: "{WorldID}:{InstanceID}"
    "position": <string>, // format: "({x}, {y}, {z})"
    "source": <string>, // always seems to be "client"
    "tags_world": <array>, // element type: <string>
    "worldId": <string>, // format: WorldID
    "worldName": <string>
}
```
All events have all these properties unless otherwise indicated.

### `$exposure`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "flag_key": <string>,
    "variant": <string>
}
```

### `Accessory_Purchase_Clicked`

This event is missing the Common Event Property `groupAccessType`.
```json
{
    "accessories": <array> [ // element type: <object>
        {
        "accessory_creator": <string?>,
        "accessory_id": <string>, // format: AvatarPartID
        "accessory_name": <string>,
        "accessory_publisher": <string>,
        "inventoryItemTemplateId": <string>, // format: TemplateID
        "inventoryItemType": <string>
        },
    ],
    "avatar_id": <string>, // format: AvatarID
    "description": <string>,
    "lastupdateddate": <string>, // format: "MM/dd/yyyy HH:mm:ss"
    "listingDisplayName": <string>,
    "listingId": <string>, // format: ListingID
    "listingSubtitle": <string>,
    "listingType": <string>,
    "listing_duration": <string>,
    "original_price_credits": <integer>,
    "price": <integer>
}
```

### `Admin_AppClose`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "_lastWorldNumberOfCustomEmojiSeen": <integer>,
    "_lastWorldNumberOfCustomEmojiSent": <integer>,
    "_lastWorldNumberOfDefaultEmojiSeen": <integer>,
    "_lastWorldNumberOfDefaultEmojiSent": <integer>,
    "_lastWorldNumberOfEmojiSeen": <integer>,
    "_lastWorldNumberOfEmojiSent": <integer>,
    "_lastWorldNumberOfExclusiveEmojiSeen": <integer>,
    "_lastWorldNumberOfExclusiveEmojiSent": <integer>,
    "_lastWorldNumberOfPremiumEmojiSeen": <integer>,
    "_lastWorldNumberOfPremiumEmojiSent": <integer>,
    "_lastWorldRadialEmojiMenuOpenCount": <integer>,
    "_lastWorldRadialItemMenuOpenCount": <integer>,
    "_lastWorldRadialMenuOpenCount": <integer>,
    "_lastWorldRadialStickerMenuOpenCount": <integer>,
    "avatarIdsEncountered": <array>, // element type: <string> // format: AvatarID
    "lastWorldBatteryLevel": <double>,
    "lastWorldBatteryLevelChange": <double>,
    "lastWorldBatteryStatus": <string>,
    "lastWorldId": <string>, // format: WorldID
    "lastWorldInstanceCode": <string>,
    "lastWorldInstanceOccupants": <integer>,
    "lastWorldInstanceType": <string>,
    "lastWorldIsHome": <boolean>,
    "lastWorldMeanTextMessageCharacters": <integer>,
    "lastWorldMicToggleCount": <string>,
    "lastWorldName": <string>,
    "lastWorldNumberOfCustomEmojiSent": <integer>,
    "lastWorldNumberOfEmojiSent": <integer>,
    "lastWorldNumberOfItemsReceivedDirectly": <integer>,
    "lastWorldNumberOfItemsReceivedViaPedestal": <integer>,
    "lastWorldNumberOfItemsSharedDirectly": <integer>,
    "lastWorldNumberOfItemsSharedViaPedestal": <integer>,
    "lastWorldNumberOfSharedItemsReported": <integer>,
    "lastWorldNumberOfStickersCreated": <integer>,
    "lastWorldNumberOfStickersPlaced": <integer>,
    "lastWorldNumberOfUsersMet": <integer>,
    "lastWorldPercentTimeSpentLoadingAvatars": <double>,
    "lastWorldPrecacheBytesDownloaded": <integer>,
    "lastWorldPrecacheCount": <integer>,
    "lastWorldPrecacheSkippedWorlds": <?>, // exact type unknown
    "lastWorldPrecachedWorlds": <?>, // exact type unknown
    "lastWorldPrivateOccupants": <integer>,
    "lastWorldPublicOccupants": <integer>,
    "lastWorldRegion": <string>,
    "lastWorldSDKVersion": <string>,
    "lastWorldStats stat {min|max|total|mean|variance|timeWeightedMean|timeWeightedVarience} {name...}": <double>, // numerous properties of this format
    "lastWorldStatusUpdatesCount": <integer>,
    "lastWorldTags_world": <array>, // element type: <string>
    "lastWorldTimeSpentInWorld": <double>,
    "lastWorldTotalTextMessageCharacters": <integer>,
    "lastWorldTotalTextMessagesSent": <integer>,
    "lastWorldTotalTimeHearingSpeech": <double>,
    "lastWorldTotalTimeSpeaking": <double>,
    "lastWorldTotalTimeSpentLoadingAvatars": <double>,
    "lastWorldUnityVersion": <string>
}
```

### `Admin_AppError`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "ActiveReason": <string>,
    "AppId": <string>,
    "Cause": <string>,
    "DurationOfSuspend": <integer>,
    "GameServerAddress": <string>,
    "LastDisconnectReason": <string>,
    "MasterServerAddress": <string>,
    "NameServerAddress": <string>,
    "NetworkClientState": <integer>,
    "PhotonTime": <double>,
    "RawCause": <string>,
    "SystemConnectionSummary": <integer?>,
    "UnityInternetReachability": <string>,
    "error": <string>,
    "reason": <string>,
    "sceneLoadErrorMessage": <string>
}
```

### `Admin_AppOpen`

This event only has the Common Event Property `source`.
```json
{
    "startupTime": <double>
}
```

### `Application_UncleanShutdownDetected`

This event only has some Common Event Properties: `source`, `worldId`, `worldName`.
```json
{
    "lastKnownContext": <string>
}
```

### `Avatar_Accessory_ViewDetails`

This event is missing the Common Event Property `groupAccessType`.
```json
{
    "accessory_creator": <string>,
    "accessory_id": <string>, // format: AvatarPartID
    "accessory_name": <string>,
    "accessory_publisher": <string>,
    "attribution": <string>,
    "category": <string>,
    "description": <string>,
    "inventoryItemTemplateId": <string>, // format: TemplateID
    "inventoryItemType": <string>,
    "lastupdateddate": <string> // format: "MM/dd/yyyy HH:mm:ss"
}
```

### `Avatar_Tab_Viewed`

This event is missing the Common Event Property `instanceTypeCombined`.

### `Avatar_ViewAll_Page_View`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "attribution": <string>,
    "avatars_clicked": <array>, // element type: <string>
    "avatars_clicked_count": <integer>,
    "avatars_seen": <array>, // element type: <string>
    "rowId": <string>,
    "rowName": <string>
}
```

### `Avatar_ViewDetails`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "attribution": <string>,
    "avatarDisplayName": <string>,
    "avatarId": <string>, // format: AvatarID
    "avatarStyle": <array>, // element type: <string>
    "avatar_platform_availability": <array>, // element type: <string>
    "listingId": <string>, // format: ListingID
    "primary_style": <string>,
    "productId": <string>, // format: ProductID
    "secondary_style": <string>,
    "sellerDisplayName": <string>,
    "sellerId": <string>,
    "tags": <array> // element type: <string>
}
```

### `Avatar_content_clicked`

This event is missing the Common Event Property `groupAccessType`.
```json
{
    "accessories": <array> [ // element type: <object>
        {
        "accessory_copies_added": <integer>,
        "accessory_creator": <string?>,
        "accessory_id": <string>, // format: AvatarPartID
        "accessory_name": <string>,
        "accessory_publisher": <string>,
        "category": <string>,
        "description": <string>,
        "inventoryItemTemplateId": <string>, // format: TemplateID
        "inventoryItemType": <string>,
        "lastupdateddate": <string> // format: date-time-instant
        },
    ],
    "accessory_creator": <string>,
    "accessory_publisher": <string>,
    "avatar_id": <string>, // format: AvatarID
    "content_type": <string>,
    "description": <string>,
    "lastupdateddate": <string>, // format: date-time-instant
    "look_id": <string>,
    "look_name": <string>,
    "ui_location": <string>,
    "user_id": <string> // format: UserID
}
```

### `Avatar_content_viewed`

This event is missing the Common Event Property `groupAccessType`.
```json
{
    "attribution": <string>,
    "ui_location": <string>,
    "user_id": <string> // format: UserID
}
```

### `Calendar_Open`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "attribution": <string>
}
```

### `Campaign_Button_Clicked`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "campaignName": <string>,
    "campaignStatusAllContributions": <integer>,
    "campaignStatusFriendParticipants": <integer>,
    "campaignStatusGoalAmount": <integer>,
    "campaignStatusGoalType": <string>,
    "campaignStatusParticipants": <integer>,
    "cta": <string>,
    "pageName": <string>,
    "userContributions": <integer>
}
```

### `Campaign_Viewed`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "attributionType": <string>,
    "campaignName": <string>,
    "campaignStatusAllContributions": <integer>,
    "campaignStatusFriendParticipants": <integer>,
    "campaignStatusGoalAmount": <integer>,
    "campaignStatusGoalType": <string>,
    "campaignStatusParticipants": <integer>,
    "menu": <string>,
    "pageName": <string>,
    "userContributions": <integer>
}
```

### `Candy_Collected`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "candy_type": <string>,
    "total_collected": <integer>
}
```

### `ColorPaletteChange`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "ID": <string>
}
```

### `Color_Profile_Saved`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "color_profile_type": <string>,
    "drone_skin_asset_bundle_id": <string>, // format: AssetBundleID
    "drone_skin_id": <string>, // format: DroneSkinID
    "drone_skin_is_default": <boolean>,
    "drone_skin_is_equipped": <boolean>,
    "drone_skin_name": <string>,
    "subscriberExclusive": <boolean>
}
```

### `Drone_Skin_Changed`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "drone_skin_asset_bundle_id": <string>, // format: AssetBundleID
    "drone_skin_id": <string>, // format: DroneSkinID
    "drone_skin_is_default": <boolean>,
    "drone_skin_name": <string>,
    "subscriberExclusive": <boolean>
}
```

### `Emoji_Spawned`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "count_nearby_users": <integer>,
    "emoji_animation_base": <string>,
    "emoji_id": <string>, // format: EmojiID
    "emoji_name": <string>,
    "emoji_type": <string>,
    "inventory_template_id": <string>, // format: TemplateID
    "is_animated": <boolean>,
    "is_premium": <boolean>,
    "spawn_source": <string>,
    "subscriberExclusive": <boolean>
}
```

### `Event_Joined`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "eventCategory": <string>,
    "eventDeletedAt": <string>, // format: date-time
    "eventDescription": <string>,
    "eventEndTime": <string>, // format: date-time
    "eventFeatured": <boolean>,
    "eventFollowing": <boolean?>,
    "eventHasImage": <boolean>,
    "eventId": <string>, // format: CalendarEventID
    "eventIsDraft": <boolean>,
    "eventLanguages": <array>, // element type: <string>
    "eventName": <string>,
    "eventOwner": <string>,
    "eventPlatforms": <array>, // element type: <string>
    "eventStartTime": <string>, // format: date-time
    "eventSupportedPlatforms": <integer>,
    "eventTags": <array>, // element type: <string>
    "eventVisibility": <integer>
}
```

### `Event_Shared`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "eventCategory": <string>,
    "eventDeletedAt": <string>, // format: date-time
    "eventDescription": <string>,
    "eventEndTime": <string>, // format: date-time
    "eventFeatured": <boolean>,
    "eventFollowing": <boolean?>,
    "eventHasImage": <boolean>,
    "eventId": <string>, // format: CalendarEventID
    "eventIsDraft": <boolean>,
    "eventLanguages": <array>, // element type: <string>
    "eventName": <string>,
    "eventOwner": <string>,
    "eventPlatforms": <array>, // element type: <string>
    "eventStartTime": <string>, // format: date-time
    "eventSupportedPlatforms": <integer>,
    "eventTags": <array>, // element type: <string>
    "eventVisibility": <integer>
}
```

### `Event_Viewed`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "eventCategory": <string>,
    "eventDeletedAt": <string>, // format: date-time
    "eventDescription": <string>,
    "eventEndTime": <string>, // format: date-time
    "eventFeatured": <boolean>,
    "eventFollowing": <boolean?>,
    "eventHasImage": <boolean>,
    "eventId": <string>, // format: CalendarEventID
    "eventIsDraft": <boolean>,
    "eventLanguages": <array>, // element type: <string>
    "eventName": <string>,
    "eventOwner": <string>,
    "eventPlatforms": <array>, // element type: <string>
    "eventStartTime": <string>, // format: date-time
    "eventSupportedPlatforms": <integer>,
    "eventTags": <array>, // element type: <string>
    "eventVisibility": <integer>
}
```

### `Event_ViewedOnWebsite`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "eventCategory": <string>,
    "eventDeletedAt": <string>, // format: date-time
    "eventDescription": <string>,
    "eventEndTime": <string>, // format: date-time
    "eventFeatured": <boolean>,
    "eventFollowing": <boolean?>,
    "eventHasImage": <boolean>,
    "eventId": <string>, // format: CalendarEventID
    "eventIsDraft": <boolean>,
    "eventLanguages": <array>, // element type: <string>
    "eventName": <string>,
    "eventOwner": <string>,
    "eventPlatforms": <array>, // element type: <string>
    "eventStartTime": <string>, // format: date-time
    "eventSupportedPlatforms": <integer>,
    "eventTags": <array>, // element type: <string>
    "eventVisibility": <integer>
}
```

### `Explore_Avatars_Viewed`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "avatars_clicked": <array>, // element type: <string>
    "avatars_clicked_count": <integer>,
    "avatars_seen": <array>, // element type: <string>
    "time_on_tab_seconds": <double>
}
```

### `Feedback_RemoveFeedback`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "category": <string>,
    "contentId": <string>, // format: some ID
    "contentName": <string>,
    "contentType": <string>,
    "reason": <string>
}
```

### `Feedback_ReportContent`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "category": <string>,
    "contentId": <string>, // format: some ID
    "contentName": <string>,
    "contentType": <string>,
    "instanceAgeGated": <boolean>,
    "reason": <string>,
    "userInSameInstance": <boolean>
}
```

### `Filter_AvatarFilter`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "attribution": <string>,
    "filter_availability": <string>,
    "filter_performance": <string>,
    "filter_platform": <string>,
    "filter_results_count": <integer>,
    "filter_selection_count": <integer>,
    "filter_stylesIds": <array>, // element type: <string>
    "filter_stylesNames": <array>, // element type: <string>
    "filter_version": <string>,
    "is_predefined_filter": <boolean>,
    "rowId": <string>,
    "rowName": <string>,
    "search_text": <string>
}
```

### `First_Action_Menu_Open`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.

### `First_Avatar_Change`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "avatarChangeMethod": <string>,
    "avatarId": <string> // format: AvatarID
}
```

### `First_Main_Menu_Open`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "pageName": <string>
}
```

### `First_Movement`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.

### `First_Quick_Menu_Open`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "pageName": <string>
}
```

### `First_Wing_Menu_Open`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "isMainMenuWing": <boolean>,
    "originPageName": <string>,
    "pageName": <string>,
    "wingType": <integer>
}
```

### `Inventory_BundleClaimed`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "bundle_name": <string>,
    "bundle_template_id": <string>, // format: TemplateID
    "item_ids": <array>, // element type: <string> // format: ItemID
    "item_names": <array>, // element type: <string>
    "item_types": <array> // element type: <string>
}
```

### `Inventory_EditCustomActionMenuGroup`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "group_renamed": <boolean>,
    "item_group_name": <string>,
    "item_ids": <array>, // element type: <string> // format: ItemID
    "item_type": <string>,
    "method": <string>
}
```

### `Inventory_MenuOpened`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "attribution": <string>
}
```

### `Loading_Screen_Changed`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "loading_screen_asset_bundle_id": <string>, // format: AssetBundleID
    "loading_screen_id": <string>,
    "loading_screen_name": <string>,
    "subscriberExclusive": <boolean>
}
```

### `Login_LoginFail`

This event only has the Common Event Property `source`.
```json
{
    "error": <string>
}
```

### `Login_LoginSuccess`

This event only has the Common Event Property `source`.

### `Login_ModerationCheckFailed`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "error": <string>
}
```

### `MainMenu_OpenVRChatPlusPage`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "ipsJourney": <array>, // element type: <string>
    "uiSource": <string>
}
```

### `MainMenu_VRChatPlus_ClickMonthly`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "uiSource": <string>
}
```

### `Marketplace_Accessories_Viewed`

This event is missing the Common Event Property `groupAccessType`.
```json
{
    "accessory_listings_clicked": <array>, // element type: <string>
    "accessory_listings_clicked_count": <integer>,
    "accessory_listings_seen": <array>, // element type: <string>
    "attribution": <string>,
    "attribution_ips_id": <string>, // format: IpsID
    "tab_exited_on": <string>,
    "time_on_tab_seconds": <double>,
    "viewing_as_admin": <boolean>
}
```

### `Marketplace_Accessory_Clicked`

This event is missing the Common Event Property `groupAccessType`.
```json
{
    "accessories": <array> [ // element type: <object>
        {
        "accessory_creator": <string?>,
        "accessory_id": <string>, // format: AvatarPartID
        "accessory_name": <string>,
        "accessory_publisher": <string>,
        "inventoryItemTemplateId": <string>, // format: TemplateID
        "inventoryItemType": <string>
        },
    ],
    "description": <string>,
    "lastupdateddate": <string>, // format: "MM/dd/yyyy HH:mm:ss"
    "listingDisplayName": <string>,
    "listingId": <string>, // format: ListingID
    "listingSubtitle": <string>,
    "listingType": <string>,
    "listing_duration": <string>,
    "original_price_credits": <integer>,
    "price": <integer>
}
```

### `Marketplace_Avatars_Viewed`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "attribution": <string>,
    "attribution_ips_id": <string>, // format: IpsID
    "avatars_clicked": <array>, // element type: <string>
    "avatars_clicked_count": <integer>,
    "avatars_seen": <array>, // element type: <string>
    "tab_exited_on": <string>,
    "time_on_tab_seconds": <double>,
    "viewing_as_admin": <boolean>
}
```

### `Marketplace_Exclusive_Viewed`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "attribution": <string>,
    "attribution_ips_id": <string>, // format: IpsID
    "listings_seen_count": <integer>,
    "listings_seen_ids": <array>, // element type: <string>
    "tab_exited_on": <string>,
    "time_on_tab_seconds": <double>,
    "viewing_as_admin": <boolean>
}
```

### `Marketplace_Groups_Store_Viewed`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "attribution": <string>,
    "attribution_ips_id": <string>, // format: IpsID
    "groups_seen_count": <integer>,
    "groups_seen_ids": <array>, // element type: <string>
    "tab_exited_on": <string>,
    "time_on_tab_seconds": <double>,
    "viewing_as_admin": <boolean>
}
```

### `Marketplace_World_Store_Viewed`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "attribution": <string>,
    "attribution_ips_id": <string>, // format: IpsID
    "authors_seen_ids": <array>, // element type: <string>
    "tab_exited_on": <string>,
    "time_on_tab_seconds": <double>,
    "viewing_as_admin": <boolean>,
    "worlds_seen_count": <integer>,
    "worlds_seen_ids": <array> // element type: <string>
}
```

### `Menu_addWorldToPlaylist`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "playlistName": <string>,
    "playlistPosition": <integer>
}
```

### `Moderation_BlockAvatar`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "avatarId": <string> // format: AvatarID
}
```

### `Moderation_BlockUser`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "targetUserId": <string> // format: UserID
}
```

### `Moderation_ChatboxMuteUser`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "targetUserId": <string> // format: UserID
}
```

### `Moderation_HideUserAvatar`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "avatarId": <string>, // format: AvatarID
    "avatarName": <string>,
    "hideGlobally": <boolean>,
    "targetUserId": <string> // format: UserID
}
```

### `Moderation_ResetShowUserAvatarToDefault`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "avatarId": <string>, // format: AvatarID
    "avatarName": <string>,
    "targetUserId": <string> // format: UserID
}
```

### `Moderation_ShowUserAvatar`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "avatarId": <string>, // format: AvatarID
    "avatarName": <string>,
    "hideGlobally": <boolean>,
    "targetUserId": <string> // format: UserID
}
```

### `Mon_ConfirmPurchase`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "attribution": <string>,
    "collab_user_id": <string>, // format: UserID
    "hasAvatar": <boolean>,
    "hasEnoughCredits": <boolean>,
    "is_first_party": <boolean>,
    "listingDisplayName": <string>,
    "listingId": <string>, // format: ListingID
    "listingType": <string>,
    "listing_duration": <string>,
    "notInWorldPurchaseError": <boolean>,
    "original_price_credits": <integer>,
    "price": <integer>,
    "products": <array> [ // element type: <object>
        {
        "productDisplayName": <string>,
        "productID": <string>, // format: ProductID
        "productType": <string>,
        "itemType": <string>,
        "inventoryItemTemplateId": <string> // format: InventoryTemplateID
        },
    ],
    "sellerId": <string>,
    "subscriberExclusive": <boolean>
}
```

### `Mon_Item_View_Details`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "inventoryItemTemplateId": <string>, // format: TemplateID
    "inventoryItemType": <string>,
    "listingDisplayName": <string>,
    "listingId": <string>, // format: ListingID
    "productDisplayName": <string>,
    "productId": <string>, // format: ProductID
    "productType": <string>,
    "productTypeLabel": <string>,
    "sellerId": <string>
}
```

### `Mon_ListingViewed`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "attribution": <string>,
    "collab_user_id": <string>, // format: UserID
    "discount_type": <string>,
    "hasAvatar": <boolean>,
    "is_first_party": <boolean>,
    "listingDisplayName": <string>,
    "listingId": <string>, // format: ListingID
    "listingType": <string>,
    "listing_duration": <string>,
    "original_price_credits": <integer>,
    "price": <integer>,
    "products": <array> [ // element type: <object>
        {
        "productDisplayName": <string>,
        "productID": <string>, // format: ProductID
        "productType": <string>,
        "itemType": <string>,
        "inventoryItemTemplateId": <string> // format: InventoryTemplateID
        },
    ],
    "sellerId": <string>,
    "subscriberExclusive": <boolean>
}
```

### `Mon_Marketplace_ViewWallet`

This event is missing the Common Event Property `groupAccessType`.
```json
{
    "attribution": <string>,
    "info": <string>,
    "locationid": <string>,
    "worldid": <string>, // format: WorldID
    "worldname": <string>
}
```

### `Mon_ReadTiliaTOS`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.

### `Mon_StorePageOpened`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "attributionType": <string>,
    "groupId": <string>, // format: GroupID
    "groupName": <string>,
    "productId": <string>, // format: ProductID
    "productName": <string>,
    "sellerId": <string>,
    "storeId": <string>, // format: StoreID
    "type": <string>
}
```

### `My_Avatars_Viewed`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.

### `Notification_Response`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "notification": <string>,
    "responseType": <string>
}
```

### `Notification_SendPhotoInvite`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "hasCustomMessage": <integer>,
    "receiverId": <string>, // format: UserID
    "receiverName": <string>,
    "type": <string>
}
```

### `Portal_Skin_Changed`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "portal_skin_id": <string>,
    "portal_skin_is_default": <boolean>,
    "portal_skin_name": <string>,
    "subscriberExclusive": <string>
}
```

### `Prints_DownloadPrint`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "authorId": <string>, // format: UserID
    "note": <string>,
    "printDate": <string>, // format: local-date
    "printWorldId": <string>, // format: WorldID
    "userId": <string>, // format: UserID
    "userIsAuthor": <boolean>
}
```

### `Prints_HidePrint`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "authorId": <string>, // format: UserID
    "note": <string>,
    "printDate": <string>, // format: local-date
    "printWorldId": <string>, // format: WorldID
    "userId": <string> // format: UserID
}
```

### `Prints_SavePrint`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "authorId": <string>, // format: UserID
    "note": <string>,
    "printDate": <string>, // format: local-date
    "printWorldId": <string>, // format: WorldID
    "userId": <string> // format: UserID
}
```

### `Promotion_Notification_Event`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "id": <string>
}
```

### `Prop_Interaction`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "inventoryItemTemplateId": <string>, // format: TemplateID
    "item_id": <string>, // format: ItemID
    "owner_id": <string>,
    "prop_id": <string>, // format: PropID
    "prop_name": <string>,
    "prop_version": <integer>
}
```

### `Prop_Removed`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "active_duration_ms": <integer>,
    "inventoryItemTemplateId": <string>, // format: TemplateID
    "item_id": <string>, // format: ItemID
    "prop_despawn_type": <string>,
    "prop_id": <string>, // format: PropID
    "prop_interactions_other": <integer>,
    "prop_interactions_spawner": <integer>,
    "prop_name": <string>,
    "prop_version": <integer>
}
```

### `Prop_Spawned`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "count_nearby_users": <integer>,
    "inventoryItemTemplateId": <string>, // format: TemplateID
    "item_id": <string>, // format: ItemID
    "prop_attachment_type": <integer>,
    "prop_creation_date": <string>,
    "prop_creator_user_id": <string>, // format: UserID
    "prop_id": <string>, // format: PropID
    "prop_interaction_types": <array>, // element type: <string>
    "prop_name": <string>,
    "prop_spawn_type": <string>,
    "prop_version": <integer>,
    "subscriberExclusive": <boolean>
}
```

### `Safety_ChangeSafetyLevel`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "newSafetyLevel": <integer>,
    "newSafetyLevelName": <string>,
    "previousSafetyLevel": <integer>,
    "previousSafetyLevelName": <string>
}
```

### `Safety_PanicModeActivated`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.

### `Search_ManualSearch`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "communityLabsEnabled": <boolean>,
    "searchArea": <string>,
    "searchCount": <integer>,
    "searchInvokedFrom": <integer>,
    "searchRefined": <boolean>,
    "searchTerm": <string>,
    "searchTermInitial": <string>,
    "searchTermSimilarity": <integer>,
    "searchType": <string>
}
```

### `Setting_DiscordLinkInitiated`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.

### `Setting_HorizonAdjustEnabled`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.

### `Setting_PersonalMirrorEnabled`

This event is missing the Common Event Property `instanceTypeCombined`.

### `Signup_AccountInfoUpdated`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`, `position`.

### `Signup_AgreeToTOS`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`, `position`.
```json
{
    "PrivacyPolicyVersion": <integer>,
    "TOSVersion": <integer>
}
```

### `Signup_ViewTOSScreen`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "PrivacyPolicyVersion": <integer>,
    "TOSVersion": <integer>
}
```

### `Social_AcceptFriendRequest`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "targetUserId": <string>, // format: UserID
    "viewedMutuals": <string>
}
```

### `Social_AddFavorite`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "id": <string>,
    "isVRCPlus_playlist": <boolean>,
    "name": <string>,
    "playlistName": <string>,
    "playlistPosition": <integer>,
    "type": <string>
}
```

### `Social_Boop`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "emojiId": <string>, // format: EmojiID
    "emojiType": <string>,
    "pageName": <string>,
    "returnBoop": <string>,
    "targetUserId": <string> // format: UserID
}
```

### `Social_CreatePrint`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "note": <string>,
    "photoResolution": <string>, // format: "{width}x{height}"
    "photoSize": <integer>,
    "photoSource": <string>,
    "userID": <string>, // format: UserID
    "worldID": <string> // format: WorldID
}
```

### `Social_CreateSticker`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "sourceID": <string>,
    "sourceType": <string>,
    "sourceURL": <string>,
    "stickerID": <string>, // format: StickerID
    "stickerURL": <string>
}
```

### `Social_InviteResponse`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "hasInviteMessage": <boolean>,
    "hasInvitePhoto": <boolean>,
    "hasViewed": <boolean>,
    "invitedWorldId": <string>, // format: WorldID
    "response": <string>,
    "senderUserId": <string>, // format: UserID
    "timeToRespond": <double>
}
```

### `Social_PlacePrint`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "authorId": <string>, // format: UserID
    "note": <string>,
    "printDate": <string>, // format: local-date
    "printWorldId": <string>, // format: WorldID
    "userId": <string> // format: UserID
}
```

### `Social_PlaceSticker`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "is_animated": <boolean>,
    "sourceUI": <string>,
    "stickerCollection": <string>,
    "stickerID": <string>, // format: StickerID
    "stickerType": <string>,
    "stickerURL": <string>,
    "subscriberExclusive": <boolean>
}
```

### `Social_ReceiveSharedContent`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "contentType": <string>,
    "sharingMode": <string>,
    "userId": <string> // format: UserID
}
```

### `Social_RemoveFavorite`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "id": <string>,
    "name": <string>,
    "type": <string>
}
```

### `Social_SendFriendRequest`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "targetUserId": <string>, // format: UserID
    "viewedDiscordFriend": <string>,
    "viewedMutuals": <string>
}
```

### `Social_ShareContent`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "contentType": <string>,
    "durationType": <string>,
    "sharingMode": <string>,
    "targets": <array>, // element type: <string>
    "userId": <string>, // format: UserID
    "visibility": <string>
}
```

### `Social_Unfriend`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "targetUserId": <string> // format: UserID
}
```

### `Social_UpdatePronouns`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "text": <string>,
    "textWasChanged": <boolean>
}
```

### `Social_UpdateStatus`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "text": <string>,
    "textWasChanged": <boolean>,
    "type": <string>,
    "typeWasChanged": <boolean>
}
```

### `Sub_EmojiUploaded`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "animation": <string>,
    "isAnimated": <boolean>
}
```

### `Sub_ProfileImageChangedClient`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "switchingToAvatarImage": <boolean>
}
```

### `SubscribeReminderClicked`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.

### `UI_IPSBannerClicked`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "attribution": <string>,
    "attributionType": <string>,
    "index": <integer>,
    "ipsBannerID": <string>,
    "ipsJourney": <array>, // element type: <string>
    "menu": <string>,
    "pageName": <string>
}
```

### `Warp_Effect_Changed`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "subscriberExclusive": <boolean>,
    "warp_effect_asset_bundle_id": <string>, // format: AssetBundleID
    "warp_effect_id": <string>, // format: WarpEffectID
    "warp_effect_name": <string>
}
```

### `World_CustomEvent`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "eventName": <string>,
    "stringPropery": <string>
}
```

### `World_EnterWorld`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "attribution": <string>,
    "attributionType": <string>,
    "avatarIdsEncountered": <array>, // element type: <string> // format: AvatarID
    "bytesDownloaded": <integer>,
    "bytesDownloadedTotal": <integer>,
    "bytesPrecached": <integer>,
    "friendsOnEnter": <integer>,
    "goButtonTime": <integer>,
    "groupId": <string>, // format: GroupID
    "groupName": <string>,
    "instanceCode": <string>,
    "instanceDidJoinDefault": <string>,
    "instanceOccupants": <integer>,
    "ipsBannersClickedCounts": <array>, // element type: <integer>
    "ipsBannersClickedIds": <array>, // element type: <string>
    "isHome": <boolean>,
    "lasWorldNumberOfCustomEmojiSent": <integer>,
    "lastLocationId": <string>,
    "lastWorldAvatarChangesCount": <integer>,
    "lastWorldAvatarChanges_ChangeMethods": <array>, // element type: <string>
    "lastWorldAvatarChanges_NewAvatarAuthorIds": <array>, // element type: <string> // format: UserID
    "lastWorldAvatarChanges_NewAvatarAuthorNames": <array>, // element type: <string>
    "lastWorldAvatarChanges_NewAvatarIds": <array>, // element type: <string> // format: AvatarID
    "lastWorldAvatarChanges_NewAvatarNames": <array>, // element type: <string>
    "lastWorldBatteryLevel": <double>,
    "lastWorldBatteryLevelChange": <double>,
    "lastWorldBatteryStatus": <string>,
    "lastWorldFallbackChangesCount": <integer>,
    "lastWorldFallbackChanges_ChangeMethods": <array>, // element type: <string>
    "lastWorldFallbackChanges_NewAvatarAuthorIds": <array>, // element type: <string> // format: UserID
    "lastWorldFallbackChanges_NewAvatarAuthorNames": <array>, // element type: <string>
    "lastWorldFallbackChanges_NewAvatarIds": <array>, // element type: <string> // format: AvatarID
    "lastWorldFallbackChanges_NewAvatarNames": <array>, // element type: <string>
    "lastWorldGroupAccessType": <string>,
    "lastWorldId": <string>, // format: WorldID
    "lastWorldImpostorsLoadedCount": <integer>,
    "lastWorldImpostorsLoadedIds": <array>, // element type: <string>
    "lastWorldImpostorsPreviewViewedCount": <integer>,
    "lastWorldImpostorsPreviewedIds": <array>, // element type: <string>
    "lastWorldInstanceCode": <string>,
    "lastWorldInstanceGroupId": <string>, // format: GroupID
    "lastWorldInstanceGroupName": <string>,
    "lastWorldInstanceOccupants": <integer>,
    "lastWorldInstanceType": <string>,
    "lastWorldIsHome": <boolean>,
    "lastWorldMeanTextMessageCharacters": <integer>,
    "lastWorldMicToggleCount": <string>,
    "lastWorldName": <string>,
    "lastWorldNumberOfEmojiSent": <integer>,
    "lastWorldNumberOfItemsReceivedDirectly": <integer>,
    "lastWorldNumberOfItemsReceivedViaPedestal": <integer>,
    "lastWorldNumberOfItemsSharedDirectly": <integer>,
    "lastWorldNumberOfItemsSharedViaPedestal": <integer>,
    "lastWorldNumberOfSharedItemsReported": <integer>,
    "lastWorldNumberOfStickersCreated": <integer>,
    "lastWorldNumberOfStickersPlaced": <integer>,
    "lastWorldNumberOfUsersMet": <integer>,
    "lastWorldPercentTimeSpentLoadingAvatars": <double>,
    "lastWorldPrecacheBytesDownloaded": <integer>,
    "lastWorldPrecacheCount": <integer>,
    "lastWorldPrecacheSkippedWorlds": <?>, // exact type unknown
    "lastWorldPrecachedWorlds": <?>, // exact type unknown
    "lastWorldPrivateOccupants": <integer>,
    "lastWorldProfilerSamples sample {max|mean|timeWeightedMean} {name...}": <double>, // numerous properties of this format
    "lastWorldPublicOccupants": <integer>,
    "lastWorldRegion": <string>,
    "lastWorldSDKVersion": <string>,
    "lastWorldStats stat {min|max|total|mean|variance|timeWeightedMean|timeWeightedVarience} {name...}": <double>, // numerous properties of this format
    "lastWorldStatusUpdatesCount": <integer>,
    "lastWorldTags_world": <array>, // element type: <string>
    "lastWorldTimeSpentInWorld": <double>,
    "lastWorldTotalTextMessageCharacters": <integer>,
    "lastWorldTotalTextMessagesSent": <integer>,
    "lastWorldTotalTimeHearingSpeech": <double>,
    "lastWorldTotalTimeSpeaking": <double>,
    "lastWorldTotalTimeSpentLoadingAvatars": <double>,
    "lastWorldUnityVersion": <string>,
    "menu": <string>,
    "pageName": <string>,
    "region": <string>,
    "transitionMenuActionID": <string>,
    "transitionPortalType": <string>,
    "transitionType": <string>,
    "userCompletedTransition": <boolean>,
    "worldDownloadingTime": <double>,
    "worldLoadingTime": <double>,
    "worldLoadingTime.{*}": <integer>, // numerous properties of this format
    "worldPrivateOccupants": <integer>,
    "worldPublicOccupants": <integer>,
    "worldTotalWaitingTime": <double>,
    "worldUdonProgramTotalBytes": <integer>,
    "worldUnityVersion": <string>
}
```

### `World_UpdateInstanceSettings`

This event is missing some Common Event Properties: `groupAccessType`, `instanceTypeCombined`.
```json
{
    "ageGate": <boolean>,
    "attribution": <string>,
    "attributionType": <string>,
    "displayName": <string>,
    "instanceFriendCount": <integer>,
    "instanceId": <string>, // format: InstanceID
    "instanceName": <string>,
    "instanceRegion": <string>,
    "instanceSetting_drones": <string>,
    "instanceSetting_emoji": <string>,
    "instanceSetting_pedestals": <string>,
    "instanceSetting_prints": <string>,
    "instanceSetting_props": <string>,
    "instanceSetting_stickers": <string>,
    "instanceUserCount": <integer>,
    "menu": <string>,
    "pageName": <string>,
    "worldAuthorId": <string>, // format: UserID
    "worldAuthorName": <string>,
    "worldCapacity": <integer>,
    "worldCreatedAt": <string>, // format: local-date-time
    "worldDefault_drones": <string>,
    "worldDefault_emoji": <string>,
    "worldDefault_pedestals": <string>,
    "worldDefault_prints": <string>,
    "worldDefault_props": <string>,
    "worldDefault_stickers": <string>,
    "worldFavoritedCount": <integer>,
    "worldHeat": <integer>,
    "worldLabsPublicationDate": <string>, // format: date-time
    "worldOccupants": <integer>,
    "worldPopularity": <integer>,
    "worldPublicationDate": <string>, // format: local-date-time
    "worldReleaseStatus": <string>,
    "worldTags": <array>, // element type: <string>
    "worldUpdatedAt": <string>, // format: local-date-time
    "worldVisits": <integer>
}
```

### `group_tab_exit`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "exit_view_group_id": <string>, // format: GroupID
    "group_impression_count": <integer>,
    "scroll": <boolean>,
    "source_entry": <string>
}
```

### `group_view_details`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "group_id": <string>, // format: GroupID
    "source_entry": <string>
}
```

### `live_now_exit`

This event is missing the Common Event Property `instanceTypeCombined`.
```json
{
    "attribution": <string>,
    "events_seen": <array>, // element type: <string>
    "exit_reason": <string>,
    "shelf_names": <array>, // element type: <string>
    "shelves_scrolled": <array>, // element type: <string>
    "shelves_seen": <string>
}
```

