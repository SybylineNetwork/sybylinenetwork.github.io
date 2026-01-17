# VRChat PlayerPrefs

VRChat Docs: [Local VRChat Storage/Windows Registry](https://docs.vrchat.com/docs/local-vrchat-storage#windows-registry)

VRChat uses the Unity [PlayerPrefs](https://docs.unity3d.com/2023.2/Documentation/ScriptReference/PlayerPrefs.html) API to store local settings and ephemeral data.

[Here](#particularly-useful-entries) is an abbreviated list of particularly useful entries.

The names of some values may contain the UserID for which the value is relevant.
The location of the inserted UserID is inconsistent, and there are sometimes both entries with and entries without a UserID.
For example, there is one settings entry that might appear reduplicated several times:
```
BACKGROUND_MATERIAL
BACKGROUND_MATERIAL_
BACKGROUND_MATERIAL_usr_c1644b5b-3ca4-45b4-97c6-a2a0de70d469
usr_c1644b5b-3ca4-45b4-97c6-a2a0de70d469_BACKGROUND_MATERIAL
usr_c1644b5b-3ca4-45b4-97c6-a2a0de70d469_BACKGROUND_MATERIAL_usr_c1644b5b-3ca4-45b4-97c6-a2a0de70d469
```
In this specific case, the last entry contains the real value; in general, if one entry's name contains a UserID and another does not, the entry whose name contains the UserID is generally canonical.
Here, entry names are "sanitized" by replacing some parts with braced text, for example:
`{UserID}_BACKGROUND_MATERIAL_{UserID}`

## Types

Although the basic types exposed by Unity are `float`, `int`, and `string`, more descriptive types will sometimes be used here:

| PlayerPrefs Type | Descriptive Type | Constraints |
| ---------------- | ---------------- | ----------- |
| `float`          |                  |             |
| `int`            | `bool`           | Either 0 (false) or 1 (true) |
| `string`         | `json`           | Valid JSON  |

### Windows peculiarities

On the Windows platform, the PlayerPrefs API uses the Windows Registry to store data.

| PlayerPrefs Type | Registry Type | Bytes | Example Registry Entry                                         | Example Value      | Notes |
| ---------------- | ------------- | ----- | -------------------------------------------------------------- | ------------------ | ----- |
| `float`          | `REG_DWORD`   | 8     | `AUDIO_MASTER_STEAMAUDIO_h477438487` `00 00 00 60 66 66 D6 3F` | 0.3499999940395355 | 64-bit IEEE 754 double-precision float, little-endian |
| `int`            | `REG_DWORD`   | 4     | `VRC_CLOCK_VARIANT_h1278072413`                  `FC 08 00 00` | 2300               | 32-bit two's-complement integer, little-endian |
| `string`         | `REG_BINARY`  | >=1   | `I2 Language_h3293684300`            `45 6E 67 6C 69 73 68 00` | English            | Modified UTF-8, NUL-terminated |

It should be explicitly noted that although Unity provides `float` as the data type exposed to applications, it stores the value in the registry as a `double` (8 bytes) with a registry type that normally has 4-byte values.
The hash/checksum suffix of the entry name (e.g., the `_h1278072413` in the second example above) is appended by Unity to avoid name collisions, as although the Windows Registry preserves the case of the names, it resolves names case-insensitive.
The algorithm used to generate the suffix is djb2-xor, as discused on the Unity forums:
```pseudocode
djb2_xor_suffix(name):
    hash = 5381
    for (char c in djb2)
        hash = (hash * 33) ^ c
    return name + "_h" + hash
```
(Archived page is available
[here](https://web.archive.org/web/20220622192844/https://answers.unity.com/questions/177945/playerprefs-changing-the-name-of-keys.html)
and [here](https://archive.ph/http://answers.unity.com/answers/208076/view.html).)

## Entries

Many of these entries may be deprecated, migrated, inoperative, or otherwise useless.

| Name                                                                | Type     | Info |
| ------------------------------------------------------------------- | -------- | ---- |
| `announcementsSeen`                                                 | `string` |      |
| *Audio levels* | | |
| `AUDIO_GAME_AVATARS`                                                | `float`  |      |
| `AUDIO_GAME_AVATARS_ENABLED`                                        | `bool`   |      |
| `AUDIO_GAME_AVATARS_STEAMAUDIO`                                     | `float`  |      |
| `AUDIO_GAME_PROPS`                                                  | `float`  |      |
| `AUDIO_GAME_PROPS_ENABLED`                                          | `bool`   |      |
| `AUDIO_GAME_PROPS_STEAMAUDIO`                                       | `float`  |      |
| `AUDIO_GAME_SFX`                                                    | `float`  |      |
| `AUDIO_GAME_SFX_ENABLED`                                            | `bool`   |      |
| `AUDIO_GAME_SFX_STEAMAUDIO`                                         | `float`  |      |
| `AUDIO_GAME_VOICE`                                                  | `float`  |      |
| `AUDIO_GAME_VOICE_ENABLED`                                          | `bool`   |      |
| `AUDIO_GAME_VOICE_STEAMAUDIO`                                       | `float`  |      |
| `AUDIO_MASTER`                                                      | `float`  |      |
| `AUDIO_MASTER_ENABLED`                                              | `bool`   |      |
| `AUDIO_MASTER_STEAMAUDIO`                                           | `float`  |      |
| `AUDIO_UI`                                                          | `float`  |      |
| `AUDIO_UI_ENABLED`                                                  | `bool`   |      |
| `AUDIO_UI_STEAMAUDIO`                                               | `float`  |      |
| `{UserID}_avatar_tab_start`                                         | `string` |      |
| `AVATAR_WORN_HISTORY_{UserID}`                                      | `json`   | `<History(AvatarID)>` |
| `{UserID}_avatarProxyAlwaysShowExplicit`                            | `bool`   |      |
| `{UserID}_avatarProxyAlwaysShowFriends`                             | `bool`   |      |
| `{UserID}_avatarProxyShowAtRange`                                   | `float`  |      |
| `{UserID}_avatarProxyShowAtRangeToggle`                             | `bool`   |      |
| `{UserID}_avatarProxyShowMaxNumber`                                 | `int`    | -1 for unlimited |
| `AVProMovieCapture-LastSavedFile`                                   | `string` | path to temporary video file |
| `BACKGROUND_DEBUG_LOG_COLLECTION`                                   | `bool`   |      |
| `{UserID}_BACKGROUND_MATERIAL_{UserID}`                             | `string` |      |
| `BestRegionCache`                                                   | `int`    | 0-3? |
| `COLOR_PALETTE_EVENT_{UserID}`                                      | `bool`   |      |
| `COLOR_PALETTES_{UserID}`                                           | `json`   | `1[ <ColorPalette>,  ... ]` The entry always seems to begin with a `1` character, the significance of which is unknown. |
| `COLOR_PALETTES_CURRENT_{UserID}`                                   | `json`   | `<ColorPalette>` |
| `CosmeticsSectionRedirect_Settings`                                 | `bool`   |      |
| `CosmeticsSectionRedirect_VRCPlus`                                  | `bool`   |      |
| `{UserID}_currentShowMaxNumberOfAvatarsEnabled`                     | `bool`   |      |
| *Custom Safety Settings*  | | `TrustLevel`: one of: `AdvancedTrustLevel1`, `AdvancedTrustLevel2`, `BasicTrustLevel1`, `BasicTrustLevel`, `DeveloperTrustLevel`, `Friend`, `IntermediateTrustLevel1`, `IntermediateTrustLevel2`, `KnownTrustLevel`, `LegendTrustLevel`, `LocalPlayer`, `NegativeTrustLevel1`, `NegativeTrustLevel2`, `NegativeTrustLevel`, `TrustedTrustLevel`, `Untrusted`, `VeryNegativeTrustLevel`, `VeteranTrustLevel` most of which are likely deprecated and may not do anything |
| `CustomTrustLevel_{TrustLevel}_CanSpeak`                            | `bool`   |      |
| `CustomTrustLevel_{TrustLevel}_CanUseAnimatedEmoji`                 | `bool`   |      |
| `CustomTrustLevel_{TrustLevel}_CanUseAvatarAudio`                   | `bool`   |      |
| `CustomTrustLevel_{TrustLevel}_CanUseCustomAnimations`              | `bool`   |      |
| `CustomTrustLevel_{TrustLevel}_CanUseCustomAvatar`                  | `bool`   |      |
| `CustomTrustLevel_{TrustLevel}_CanUseCustomShaders`                 | `bool`   |      |
| `CustomTrustLevel_{TrustLevel}_CanUseDrone`                         | `bool`   |      |
| `CustomTrustLevel_{TrustLevel}_CanUseEmojiStickersSharing`          | `bool`   |      |
| `CustomTrustLevel_{TrustLevel}_CanUseTriggers`                      | `bool`   |      |
| `CustomTrustLevel_{TrustLevel}_CanUseUserIcons`                     | `bool`   |      |
| `Dashboard_Emojis_Foldout{1-8}`                                     | `bool`   |      |
| `DEFAULT_GROUP_{UserID}`                                            | `string` |      |
| `DefaultWorldList_{UserID}`                                         | `string` |      |
| `{UserID}_DroneControllerSettings`                                  | `json`   | `<DroneControllerSettings>` |
| `{UserID}_DroneFlightPresetValues`                                  | `json`   | `<DroneFlightPresetValues>` |
| `{UserID}_enableSliderSnapping`                                     | `bool`   |      |
| `expiredSeenReminderPopup{UserID}`                                  | `string` | `MM/dd/yyyy HH:mm:ss`: [DateTime.ToString(CultureInfo.InvariantCulture)](https://learn.microsoft.com/en-us/dotnet/api/system.datetime.tostring) |
| `FaceMirrorOwner`                                                   | `int`    |      |
| `FIELD_OF_VIEW`                                                     | `float`  |      |
| `FOLDOUT_STATES`                                                    | `string` | `<integer>(,<integer>)*` |
| `ForceSettings_ClearFoldoutPrefKeys`                                | `int`    |      |
| `ForceSettings_DisableGuidedMode`                                   | `int`    |      |
| `ForceSettings_MicTalk`                                             | `int`    |      |
| `ForceSettings_MicToggle`                                           | `int`    |      |
| `ForceSettings_MigrateMicSettings`                                  | `int`    |      |
| `ForceSettings_Mixer`                                               | `int`    |      |
| `ForceSettings_PedestalSharing`                                     | `int`    |      |
| `ForceSettings_SteamAudioSliderRemap`                               | `int`    |      |
| `ForceSettings_Tutorial`                                            | `int`    |      |
| `ForceSettings_WorldTooltipMode`                                    | `int`    |      |
| `FPS_LIMIT`                                                         | `int`    |      |
| `FPSCapType`                                                        | `int`    |      |
| `FPSType`                                                           | `int`    |      |
| `FRIEND_LAST_VISIT_HISTORY_{UserID}`                                | `json`   | `<History(UserID)>` |
| `{UserID}_GiftDropNoRefundAfterUsageAgreeToggle`                    | `bool`   |      |
| *"Has Seen" flags* | | |
| `{UserID}_has_seen_avm-explore-to-cm-migration`                     | `bool`   |      |
| `{UserID}_has_seen_event_discovery_in_beta`                         | `bool`   |      |
| `HasSeen_Callout_AVM_Explore_{UserID}`                              | `bool`   |      |
| `HasSeen_Callout_VRCPlusNewFeatures_{UserID}`                       | `bool`   |      |
| `HasSeen_Callout_AVM_Explore_{UserID}`                              | `bool`   |      |
| `HasSeen{*}{Callout|Popup}{UserID}`                                 | `bool`   |      |
| `HUD_SETTINGS`                                                      | `json`   | `<HudSettings>` |
| `HUD_SMOOTHING_DESKTOP`                                             | `bool`   |      |
| `HUD_SMOOTHING_VR`                                                  | `bool`   |      |
| `I2 Language`                                                       | `string` |      |
| `{UserID}_InPageCallout_Drone`                                      | `int`    |      |
| `InQueueWidgetInfoShowcaseID`                                       | `int`    |      |
| `INSTANCE_RENAME_HISTORY_{UserID}`                                  | `json`   | `<History(string)>` |
| `{UserID}_LastExpiredSubscription`                                  | `string` |      |
| `LastHudMode`                                                       | `int`    |      |
| `LocalPlayerMods_{UserID}`                                          | `json`   | `{ "<UserID>": [ <integer> ], ... }` |
| `LocalPlayerMods_{UserID}_Migrated`                                 | `json`   | `[ <integer>, <integer> ]` |
| `LocationContext`                                                   | `int`    | 0-2  |
| `LocationContext_World`                                             | `string` | `<WorldID>|<string>`, where the last string is the world name |
| `LOD_QUALITY`                                                       | `int`    |      |
| `LOGGING_ENABLED`                                                   | `bool`   |      |
| `migrated-local-pmods-{UserID}-HideAvatar`                          | `bool`   |      |
| `migrated-local-pmods-{UserID}-HideDrone`                           | `bool`   |      |
| `migrated-local-pmods-{UserID}-ShowAvatar`                          | `bool`   |      |
| `migrated-local-pmods-{UserID}-ShowDrone`                           | `bool`   |      |
| *Main Menu UI State* | | |
| `MM.Dashboard.RecentlyUpdatedFavoriteAvatars`                       | `int`    |      |
| `MM.Dashboard.RecentlyUpdatedFavoriteWorlds`                        | `int`    |      |
| `MM.Dashboard.RecentlyVisitedOnlineFriends`                         | `int`    |      |
| `MM.Dashboard.RecentlyVisitedUsers`                                 | `int`    |      |
| `MM.Dashboard.RecentWorlds`                                         | `int`    |      |
| `MM.Settings.Accessibility.Chatbox`                                 | `int`    |      |
| `MM.Settings.Accessibility.Comfort`                                 | `int`    |      |
| `MM.Settings.Accessibility.Display`                                 | `int`    |      |
| `MM.Settings.Accessibility.Haptics`                                 | `int`    |      |
| `MM.Settings.Audio.MicBehavior`                                     | `int`    |      |
| `MM.Settings.Audio.Microphone`                                      | `int`    |      |
| `MM.Settings.Audio.Other`                                           | `int`    |      |
| `MM.Settings.Audio.Volume`                                          | `int`    |      |
| `MM.Settings.AudioAndVoice.Audio`                                   | `int`    |      |
| `MM.Settings.AudioAndVoice.Earmuff`                                 | `int`    |      |
| `MM.Settings.AudioAndVoice.Microphone`                              | `int`    |      |
| `MM.Settings.Avatars.Culling`                                       | `int`    |      |
| `MM.Settings.Avatars.Interactions`                                  | `int`    |      |
| `MM.Settings.Avatars.Optimizations`                                 | `int`    |      |
| `MM.Settings.Avatars.Yours`                                         | `int`    |      |
| `MM.Settings.ComfortAndSafety.Comfort`                              | `int`    |      |
| `MM.Settings.ComfortAndSafety.FingerTracking`                       | `int`    |      |
| `MM.Settings.ComfortAndSafety.Safety`                               | `int`    |      |
| `MM.Settings.Debug.Debug`                                           | `int`    |      |
| `MM.Settings.Debug.ManageCachedData`                                | `int`    |      |
| `MM.Settings.Debug.YourAvatar`                                      | `int`    |      |
| `MM.Settings.Grahics.Display`                                       | `int`    |      |
| `MM.Settings.Graphics.Common`                                       | `int`    |      |
| `MM.Settings.Graphics.Quality`                                      | `int`    |      |
| `MM.Settings.Mirrors.Face`                                          | `int`    |      |
| `MM.Settings.Mirrors.Personal`                                      | `int`    |      |
| `MM.Settings.Performance.Advanced`                                  | `int`    |      |
| `MM.Settings.Performance.BlockAvatars`                              | `int`    |      |
| `MM.Settings.Performance.DownloadPrioritization`                    | `int`    |      |
| `MM.Settings.Performance.DynamicBones`                              | `int`    |      |
| `MM.Settings.Performance.ManageCachedData`                          | `int`    |      |
| `MM.Settings.Performance.MaxAvatarSize`                             | `int`    |      |
| `MM.Settings.TrackingAndIK.TrackingAndIK`                           | `int`    |      |
| `MM.Settings.TrackingAndIk.TrackingAndIk`                           | `int`    |      |
| `MM.Settings.UserInterface.ActionMenu`                              | `int`    |      |
| `MM.Settings.UserInterface.BackgroundDesigns`                       | `int`    |      |
| `MM.Settings.UserInterface.Chatbox`                                 | `int`    |      |
| `MM.Settings.UserInterface.General`                                 | `int`    |      |
| `MM.Settings.UserInterface.GeneralSettings`                         | `int`    |      |
| `MM.Settings.UserInterface.HUD`                                     | `int`    |      |
| `MM.Settings.UserInterface.HUD.VerboseMode`                         | `int`    |      |
| `MM.Settings.UserInterface.HUDVerbose`                              | `int`    |      |
| `MM.Settings.UserInterface.Nameplate`                               | `int`    |      |
| `MM.Settings.UserInterface.Nameplates`                              | `int`    |      |
| `MM.Settings.UserInterface.Visuals`                                 | `int`    |      |
| `MM.Settings.UserInterface.Visuals.BAckgroundOptions`               | `int`    |      |
| `MM.Worlds.MyWorlds`                                                | `int`    |      |
| `MostRecentSubId{UserID}`                                           | `string` | `<TransactionID>` |
| `{UserID}_OpenedQuickMenu`                                          | `int`    |      |
| `{UserID}_OpenMenuHelpShownCount`                                   | `int`    |      |
| `PARTICLE_PHYSICS_QUALITY`                                          | `int`    |      |
| `{UserID}_LastExpiredSubscription`                                  | `string` |      |
| `{UserID}_LastExpiredSubscription`                                  | `string` |      |
| `PIXEL_LIGHT_COUNT`                                                 | `int`    |      |
| *Personal Mirror Settings* | | |
| `PersonalMirror.FaceMirrorOpacity`                                  | `float`  |      |
| `PersonalMirror.FaceMirrorOpacityDesktop`                           | `float`  |      |
| `PersonalMirror.FaceMirrorPosX`                                     | `float`  |      |
| `PersonalMirror.FaceMirrorPosXDesktop`                              | `float`  |      |
| `PersonalMirror.FaceMirrorPosY`                                     | `float`  |      |
| `PersonalMirror.FaceMirrorPosYDesktop`                              | `float`  |      |
| `PersonalMirror.FaceMirrorScale`                                    | `float`  |      |
| `PersonalMirror.FaceMirrorScaleDesktop`                             | `float`  |      |
| `PersonalMirror.FaceMirrorSpoutEnabled`                             | `bool`   |      |
| `PersonalMirror.FaceMirrorZoom`                                     | `float`  |      |
| `PersonalMirror.FaceMirrorZoomDesktop`                              | `float`  |      |
| `PersonalMirror.Grabbable`                                          | `bool`   |      |
| `PersonalMirror.ImmersiveMove`                                      | `bool`   |      |
| `PersonalMirror.MirrorOpacity`                                      | `float`  |      |
| `PersonalMirror.MirrorScale`                                        | `float`  |      |
| `PersonalMirror.MirrorScaleX`                                       | `float`  |      |
| `PersonalMirror.MirrorScaleY`                                       | `float`  |      |
| `PersonalMirror.MirrorSnapping`                                     | `bool`   |      |
| `PersonalMirror.MirrorTransparency`                                 | `float`  |      |
| `PersonalMirror.MovementMode`                                       | `int`    |      |
| `PersonalMirror.ShowBorder`                                         | `bool`   |      |
| `PersonalMirror.ShowCalibrationMirror`                              | `bool`   |      |
| `PersonalMirror.ShowEnvironmentInMirror`                            | `bool`   |      |
| `PersonalMirror.ShowFaceMirror`                                     | `bool`   |      |
| `PersonalMirror.ShowFaceMirrorDesktop`                              | `bool`   |      |
| `PersonalMirror.ShowHudMirror`                                      | `bool`   |      |
| `PersonalMirror.ShowRemotePlayerInMirror`                           | `bool`   |      |
| `PersonalMirror.ShowUIInMirror`                                     | `bool`   |      |
| `PersonalMirror.WorldSpaceMirror`                                   | `bool`   |      |
| `PlayerHeight`                                                      | `float`  | meters |
| `PREF_HAND_TRACKING_TUTORIAL_COMPLETED`                             | `bool`   |      |
| `Preload_Worlds_HISTORY_{UserID}`                                   | `json`   | `<History(WorldID)>` |
| `{UserID}_PromotionData`                                            | `json`   | `{ "PromotionDataHandlers": [ { "<string>": <any>, ... }, ... ] }` |
| *Quick Menu UI State* | | |
| `QM.Audio.EarmuffMode`                                              | `bool`   |      |
| `QM.Audio.Microphone`                                               | `int`    |      |
| `QM.Audio.Volume`                                                   | `int`    |      |
| `QM.Here.BlockedByUsers`                                            | `int`    |      |
| `QM.Here.BlockedUsers`                                              | `int`    |      |
| `QM.Here.UsersInWorld`                                              | `int`    |      |
| `QM.Here.WorldActions`                                              | `int`    |      |
| `QM.SelectedUser.DevTools`                                          | `int`    |      |
| `QM.SelectedUser.InstanceModeratorActions`                          | `int`    |      |
| `QM.SelectedUser.PerUseInteraction`                                 | `int`    |      |
| `QM.SelectedUser.UserActions`                                       | `int`    |      |
| `QM.Settings.Accessibility`                                         | `int`    |      |
| `QM.Settings.AvatarInteractions`                                    | `int`    |      |
| `QM.Settings.Chatbox`                                               | `int`    |      |
| `QM.Settings.Debug`                                                 | `int`    |      |
| `QM.Settings.Display`                                               | `int`    |      |
| `QM.Settings.Performance`                                           | `int`    |      |
| `QM.Settings.TrackingAndIK`                                         | `int`    |      |
| `QM.Settings.UIElements`                                            | `int`    |      |
| `RECENTLY_VISITED_HISTORY_{UserID}`                                 | `json`   | `<History(UserID)>` |
| `SavedWorldSearches{UserID}`                                        | `json`   | `{ "Searches": [ ... ] }` |
| `Screenmanager Fullscreen mode`                                     | `int`    |      |
| `Screenmanager Fullscreen mode Default`                             | `int`    |      |
| `Screenmanager Resolution Height`                                   | `int`    |      |
| `Screenmanager Resolution Height Default`                           | `int`    |      |
| `Screenmanager Resolution Use Native`                               | `bool`   |      |
| `Screenmanager Resolution Use Native Default`                       | `bool`   |      |
| `Screenmanager Resolution Width`                                    | `int`    |      |
| `Screenmanager Resolution Width Default`                            | `int`    |      |
| `Screenmanager Resolution Window Height`                            | `int`    |      |
| `Screenmanager Resolution Window Width`                             | `int`    |      |
| `Screenmanager Stereo 3D`                                           | `bool`   |      |
| `Screenmanager Window Position X`                                   | `int`    |      |
| `Screenmanager Window Position Y`                                   | `int`    |      |
| `SeatedPlayEnabled`                                                 | `bool`   |      |
| `SeenIPS_SEEN_{UserID}`                                             | `json`   | `[ <string> ]` |
| `Settings.HUD.VerboseFoldout`                                       | `bool`   |      |
| `Settings.Visuals.BackgroundsFoldout`                               | `bool`   |      |
| `SHADOW_QUALITY`                                                    | `int`    |      |
| `SHADOW_RESOLUTION_QUALITY`                                         | `int`    |      |
| `ShowCalibrationMirror`                                             | `bool`   |      |
| `SortSelection_{*}`                                                 | `int`    |      |
| `timeFormatIs12hrs`                                                 | `bool`   |      |
| `timeFormatIs12hrs_migrated`                                        | `bool`   |      |
| *Calendar Filter Toggles* | | |
| `TOGGLE_STATE_FilterCalendarEventsModal_EventAccessType_{*}`        | `bool`   |      |
| `TOGGLE_STATE_FilterCalendarEventsModal_EventCategory_{*}`          | `bool`   |      |
| `TOGGLE_STATE_FilterCalendarEventsModal_EventDay_{*}`               | `bool`   |      |
| `TOGGLE_STATE_FilterCalendarEventsModal_EventSource_{*}`            | `bool`   |      |
| `TOGGLE_STATE_FilterCalendarEventsModal_{GroupID}`                  | `bool`   |      |
| `TOGGLE_STATE_FilterCalendarEventsModal_Language_{*}`               | `bool`   |      |
| `TOGGLE_STATE_FilterCalendarEventsModal_Platform_{*}`               | `bool`   |      |
| *UI State* | | |
| `UI.{UserID}.DragPanelTutorialComplete`                             | `bool`   |      |
| `{UserID}_UI.Emojis.CustomGroup{0-2}`                               | `json`   | `<CustomGroup(ItemID)>` |
| `UI.HereMenu.BlockedByUsers`                                        | `int`    |      |
| `UI.HereMenu.BlockedUsers`                                          | `int`    |      |
| `UI.HereMenu.Users`                                                 | `int`    |      |
| `UI.HereMenu.WorldActions`                                          | `int`    |      |
| `UI.MainMenuSocialPage.BlockedByUsers`                              | `int`    |      |
| `UI.MainMenuSocialPage.BlockedUsers`                                | `int`    |      |
| `UI.MenuPlacementZDepthVR`                                          | `float`  |      |
| `UI.MotionSmoothingEnabled`                                         | `bool`   |      |
| `{UserID}_UI.Props.CustomGroup{0}`                                  | `json`   | `<CustomGroup(ItemID)>` |
| `UI.RecentlyUsedEmojiNames`                                         | `string` | `<FileID>(,<FileID>)*` |
| `UI.RecentlyUsedEmojiNames.Count`                                   | `int`    |      |
| `UI.RecentlyUsedEmojiNames{0-9}`                                    | `string` |      |
| `{UserID}_UI.RecentlyUsedEmojis`                                    | `json`   | `<CustomGroup(ItemID)>` |
| `{UserID}_UI.RecentlyUsedStickers`                                  | `json`   | `<CustomGroup(ItemID)>` |
| `UI.RecentlyUsedStickers`                                           | `string` | `<FileID>(,<FileID>)*` |
| `UI.RecentlyUsedStickers.Count`                                     | `int`    |      |
| `UI.SelectedUser.DevTools`                                          | `int`    |      |
| `UI.SelectedUser.InstanceModeratorActions`                          | `int`    |      |
| `UI.SelectedUser.PerUseInteraction`                                 | `int`    |      |
| `UI.SelectedUser.UserActions`                                       | `int`    |      |
| `UI.Settings.Accessibility`                                         | `int`    |      |
| `UI.Settings.AdvancedOptions`                                       | `int`    |      |
| `UI.Settings.AvatarPerformance`                                     | `int`    |      |
| `UI.Settings.AvInteractions`                                        | `int`    |      |
| `UI.Settings.Chatbox`                                               | `int`    |      |
| `UI.Settings.Comfort`                                               | `int`    |      |
| `UI.Settings.Debug`                                                 | `int`    |      |
| `UI.Settings.FBT`                                                   | `int`    |      |
| `UI.Settings.Haptics`                                               | `int`    |      |
| `UI.Settings.Osc`                                                   | `int`    |      |
| `UI.Settings.Safety`                                                | `int`    |      |
| `UI.Settings.TrackingAndIk`                                         | `int`    |      |
| `UI.Settings.UIElements`                                            | `int`    |      |
| `UI.Settings.VisualAdjustments`                                     | `int`    |      |
| `{UserID}_UI.Stickers.CustomGroup{0-2}`                             | `json`   | `<CustomGroup(ItemID)>` |
| `unity.cloud_userid`                                                | `string` | unity internal? |
| `unity.player_session_count`                                        | `int`    | unity internal? |
| `unity.player_sessionid`                                            | `string` | unity internal? |
| `UnityGraphicsQuality`                                              | `int`    | unity internal? |
| `UnitySelectMonitor`                                                | `int`    | unity internal? |
| *Camera Settings* | | |
| `USER_CAMERA_RESOLUTION`                                            | `string` |      |
| `USER_CAMERA_ROLL_WHILE_FLYING`                                     | `bool`   |      |
| `USER_CAMERA_SAVE_METADATA`                                         | `bool`   |      |
| `USER_CAMERA_STREAM_RESOLUTION`                                     | `int`    |      |
| `USER_CAMERA_TRIGGER_TAKES_PHOTOS`                                  | `bool`   |      |
| `UserId`                                                            | `string` | unity internal? |
| *General Settings* | | |
| `VRC.UI.QuickMenu.ShowQMDebugInfo`                                  | `bool`   |      |
| `VRC_ACTION_MENU_FLICK_SELECT`                                      | `bool`   |      |
| `VRC_ACTION_MENU_ONE_HAND_MOVE`                                     | `bool`   |      |
| `VRC_ACTION_MENU_{L\|R}_HUD_ANGLE_X`                                | `float`  |      |
| `VRC_ACTION_MENU_{L\|R}_HUD_ANGLE_Y`                                | `float`  |      |
| `VRC_ACTION_MENU_{L\|R}_MENU_OPACITY`                               | `float`  |      |
| `VRC_ACTION_MENU_{L\|R}_MENU_SIZE`                                  | `int`    |      |
| `VRC_ACTION_MENU_{L\|R}_MENU_SIZE_PERCENTAGE`                       | `float`  |      |
| `VRC_ACTION_MENU_{L\|R}_SHOW_ON_HUD`                                | `bool`   |      |
| `VRC_ADVANCED_GRAPHICS_ANTIALIASING`                                | `int`    |      |
| `VRC_ADVANCED_GRAPHICS_QUALITY`                                     | `string` |      |
| `VRC_AFK_ENABLED`                                                   | `bool`   |      |
| `VRC_ALLOW_AVATAR_COPYING`                                          | `bool`   |      |
| `VRC_ALLOW_DIRECT_SHARES`                                           | `bool`   |      |
| `VRC_ALLOW_FOCUS_VIEW`                                              | `bool`   |      |
| `VRC_ALLOW_PEDESTAL_SHARES`                                         | `bool`   |      |
| `VRC_ALLOW_PRINTS`                                                  | `bool`   |      |
| `VRC_ALLOW_SHARED_CONNECTIONS`                                      | `bool`   |      |
| `VRC_ALLOW_UNTRUSTED_URL`                                           | `bool`   |      |
| `VRC_ANDROID_MIC_MODE`                                              | `int`    |      |
| `VRC_ASK_TO_PORTAL`                                                 | `bool`   |      |
| `VRC_AV_INTERACT_LEVEL`                                             | `bool`   |      |
| `VRC_AV_INTERACT_SELF`                                              | `bool`   |      |
| `VRC_AVATAR_FALLBACK_HIDDEN`                                        | `bool`   |      |
| `VRC_AVATAR_HAPTICS_ENABLED`                                        | `bool`   |      |
| `VRC_AVATAR_MAXIMUM_DOWNLOAD_SIZE`                                  | `int`    |      |
| `VRC_AVATAR_MAXIMUM_UNCOMPRESSED_SIZE`                              | `int`    |      |
| `VRC_AVATAR_PERFORMANCE_RATING_MINIMUM_TO_DISPLAY`                  | `int`    |      |
| `VRC_BINDING_STEAMVR_INDEX_HOLD_QM`                                 | `int`    |      |
| `VRC_BINDING_STEAMVR_INDEX_L_ABUTTON_PRESS`                         | `int`    |      |
| `VRC_BINDING_STEAMVR_INDEX_L_JOY_PRESS`                             | `int`    |      |
| `VRC_BINDING_STEAMVR_INDEX_LT_GRAB`                                 | `int`    |      |
| `VRC_BINDING_STEAMVR_INDEX_PRESET`                                  | `int`    |      |
| `VRC_BINDING_STEAMVR_INDEX_R_ABUTTON_PRESS`                         | `int`    |      |
| `VRC_BINDING_STEAMVR_INDEX_R_JOY_PRESS`                             | `int`    |      |
| `VRC_BLOOM_INTENSITY`                                               | `float`  |      |
| `VRC_BOOP_EMOJI_EFFECT_ENABLED`                                     | `bool`   |      |
| `VRC_CAMERA_NEAR_CLIP_OVERRIDE_MODE`                                | `int`    |      |
| `VRC_CAMERA_THIRD_PERSON_VIEW`                                      | `int`    |      |
| `VRC_CAMERA_THIRD_PERSON_VIEW_DISTANCE`                             | `float`  |      |
| `VRC_CHAT_BUBBLE_ABOVE_HEAD`                                        | `bool`   |      |
| `VRC_CHAT_BUBBLE_ABOVE_HEAD_V2`                                     | `bool`   |      |
| `VRC_CHAT_BUBBLE_AUDIO_ENABLED`                                     | `bool`   |      |
| `VRC_CHAT_BUBBLE_AUDIO_VOLUME`                                      | `float`  |      |
| `VRC_CHAT_BUBBLE_AUTO_SEND`                                         | `bool`   |      |
| `VRC_CHAT_BUBBLE_OPACITY`                                           | `float`  |      |
| `VRC_CHAT_BUBBLE_POS_HEIGHT`                                        | `float`  |      |
| `VRC_CHAT_BUBBLE_PROFANITY_FILTER`                                  | `bool`   |      |
| `VRC_CHAT_BUBBLE_SCALE`                                             | `float`  |      |
| `VRC_CHAT_BUBBLE_SHOW_OWN`                                          | `bool`   |      |
| `VRC_CHAT_BUBBLE_TIMEOUT`                                           | `float`  |      |
| `VRC_CHAT_BUBBLE_VISIBILITY`                                        | `bool`   |      |
| `VRC_CLEAR_CACHE_ON_START`                                          | `bool`   |      |
| `VRC_CLOCK_HOUR_MODE`                                               | `int`    |      |
| `VRC_CLOCK_TIMEZONE_MODE`                                           | `int`    |      |
| `VRC_CLOCK_VARIANT`                                                 | `int`    |      |
| `VRC_COLOR_BLINDNESS_SIMULATE`                                      | `bool`   |      |
| `VRC_COLOR_FILTER_INTENSITY`                                        | `float`  |      |
| `VRC_COLOR_FILTER_SELECTION`                                        | `int`    |      |
| `VRC_COLOR_FILTER_TO_WORLD`                                         | `bool`   |      |
| `VRC_COMFORT_MODE`                                                  | `bool`   |      |
| `VRC_COMFORT_MODE_DESKTOP`                                          | `bool`   |      |
| `VRC_COMFORT_MODE_PRE_HOLOPORT`                                     | `bool`   |      |
| `VRC_CONVERT_ALL_DYNAMIC_BONES`                                     | `bool`   |      |
| `VRC_CURRENT_LANGUAGE`                                              | `string` |      |
| `VRC_DEFAULT_DRONE_SKIN_PALETTE`                                    | `json`   | `{ "Current": "#2a4bfc,#38bbff,#0076ff", "Default": "#2a4bfc,#38bbff,#0076ff" }` |
| `VRC_DESKTOP_RETICLE`                                               | `bool`   |      |
| `VRC_DIRECT_SHARING_VISIBILITY`                                     | `bool`   |      |
| `VRC_DISABLE_AVATAR_CLONING_ON_ENTER_WORLD`                         | `bool`   |      |
| `VRC_DOUBLE_TAP_MAIN_MENU_STEAMVR2`                                 | `bool`   |      |
| `VRC_DOWNLOAD_PRIORITIZE_DISTANCE_ENABLED`                          | `bool`   |      |
| `VRC_EARMUFF_MODE`                                                  | `bool`   |      |
| `VRC_EARMUFF_MODE_AVATARS`                                          | `bool`   |      |
| `VRC_EARMUFF_MODE_CONE_VALUE`                                       | `float`  |      |
| `VRC_EARMUFF_MODE_FALLOFF`                                          | `float`  |      |
| `VRC_EARMUFF_MODE_FOLLOW_HEAD`                                      | `bool`   |      |
| `VRC_EARMUFF_MODE_LOCK_ROTATION`                                    | `bool`   |      |
| `VRC_EARMUFF_MODE_OFFSET_VALUE`                                     | `float`  |      |
| `VRC_EARMUFF_MODE_RADIUS`                                           | `float`  |      |
| `VRC_EARMUFF_MODE_REDUCED_VOLUME`                                   | `float`  |      |
| `VRC_EARMUFF_MODE_SHOW_ICON_IN_NAMEPLATE`                           | `bool`   |      |
| `VRC_EARMUFF_MODE_VISUAL_AIDE`                                      | `bool`   |      |
| `VRC_ENABLE_GROUP_INTEREST_AUTO_NOTIFICATIONS`                      | `bool`   |      |
| `VRC_FINGER_GRAB_SETTING`                                           | `bool`   |      |
| `VRC_FINGER_HAPTIC_SENSITIVITY`                                     | `float`  |      |
| `VRC_FINGER_HAPTIC_STRENGTH`                                        | `float`  |      |
| `VRC_FINGER_JUMP_ENABLED`                                           | `bool`   |      |
| `VRC_FINGER_WALK_SETTING`                                           | `int`    |      |
| `VRC_FINGERTRACKING_AVATARS_USE`                                    | `bool`   |      |
| `VRC_FINGERTRACKING_SHOW_PINCHUI`                                   | `bool`   |      |
| `VRC_FINGERTRACKING_USE_EXCLUSIVE`                                  | `bool`   |      |
| `VRC_FINGERTRACKING_USE_GHOSTHAND`                                  | `bool`   |      |
| `VRC_GESTURE_BAR_ENABLED`                                           | `bool`   |      |
| `VRC_GROUP_ON_NAMEPLATE`                                            | `string` | `<GroupID>` |
| `VRC_GROUP_ORDER_{UserID}`                                          | `json`   | `[ <GroupID> ]` |
| `VRC_GUIDED_MODE_OPT_IN`                                            | `bool`   |      |
| `VRC_GUIDED_MODE_STATE`                                             | `int`    |      |
| `VRC_HANDS_MENU_OPEN_MODE`                                          | `int`    |      |
| `VRC_HIDE_NOTIFICATION_PHOTOS`                                      | `bool`   |      |
| `VRC_HOME_ACCESS_TYPE`                                              | `int`    |      |
| `VRC_HOME_REGION`                                                   | `int`    |      |
| `VRC_HUD_ANCHOR`                                                    | `int`    |      |
| `VRC_HUD_MIC_OPACITY`                                               | `float`  |      |
| `VRC_HUD_MODE`                                                      | `int`    |      |
| `VRC_HUD_OPACITY`                                                   | `float`  |      |
| `VRC_HUD_SMOOTHING_DESKTOP`                                         | `bool`   |      |
| `VRC_HUD_SMOOTHING_Desktop`                                         | `bool`   |      |
| `VRC_HUD_SMOOTHING_VR`                                              | `bool`   |      |
| `VRC_IK_AVATAR_MEASUREMENT_TYPE`                                    | `int`    |      |
| `VRC_IK_CALIBRATION_RANGE`                                          | `float`  |      |
| `VRC_IK_CALIBRATION_VIS`                                            | `bool`   |      |
| `VRC_IK_DEBUG_LOGGING`                                              | `bool`   |      |
| `VRC_IK_DISABLE_SHOULDER_TRACKING`                                  | `bool`   |      |
| `VRC_IK_FBT_CONFIRM_CALIBRATE`                                      | `bool`   |      |
| `VRC_IK_FBT_LOCOMOTION`                                             | `bool`   |      |
| `VRC_IK_FBT_SPINE_MODE`                                             | `int`    |      |
| `VRC_IK_FREEZE_TRACKING_ON_DISCONNECT`                              | `bool`   |      |
| `VRC_IK_HEIGHT_RATIO`                                               | `float`  |      |
| `VRC_IK_KNEE_ANGLE`                                                 | `float`  |      |
| `VRC_IK_LEGACY`                                                     | `bool`   |      |
| `VRC_IK_LEGACY_CALIBRATION`                                         | `bool`   |      |
| `VRC_IK_ONE_HANDED_CALIBRATION`                                     | `bool`   |      |
| `VRC_IK_PER_AVATAR_CALIBRATION_ADJUSTMENT`                          | `bool`   |      |
| `VRC_IK_SHOULDER_WIDTH_COMPENSATION`                                | `bool`   |      |
| `VRC_IK_TRACKER_MODEL`                                              | `int`    |      |
| `VRC_IK_USE_METRIC_HEIGHT`                                          | `bool`   |      |
| `VRC_IK_WRIST_ANGLE`                                                | `float`  |      |
| `VRC_IMPOSTOR_WHEN_AVAILABLE`                                       | `bool`   |      |
| `VRC_INPUT_COMFORT_TURNING`                                         | `int`    |      |
| `VRC_INPUT_CONTROLLER`                                              | `int`    |      |
| `VRC_INPUT_DAYDREAM`                                                | `int`    |      |
| `VRC_INPUT_DISABLE_MIC_BUTTON`                                      | `int`    |      |
| `VRC_INPUT_EMBODIED`                                                | `int`    |      |
| `VRC_INPUT_GAZE`                                                    | `int`    |      |
| `VRC_INPUT_GENERIC`                                                 | `int`    |      |
| `VRC_INPUT_HEAD_LOOK_WALK`                                          | `int`    |      |
| `VRC_INPUT_HPMOTION`                                                | `int`    |      |
| `VRC_INPUT_KEYBOARD`                                                | `int`    |      |
| `VRC_INPUT_LOCOMOTION_METHOD`                                       | `int`    |      |
| `VRC_INPUT_MIC_DEVICE_NAME`                                         | `string` |      |
| `VRC_INPUT_MIC_DEVICE_NAME_Desktop`                                 | `string` |      |
| `VRC_INPUT_MIC_DEVICE_NAME_VR`                                      | `string` |      |
| `VRC_INPUT_MIC_ENABLED`                                             | `bool`   |      |
| `VRC_INPUT_MIC_LEVEL_DESK`                                          | `float`  |      |
| `VRC_INPUT_MIC_LEVEL_VR`                                            | `float`  |      |
| `VRC_INPUT_MIC_MODE`                                                | `int`    |      |
| `VRC_INPUT_MIC_NOISE_GATE`                                          | `float`  |      |
| `VRC_INPUT_MIC_NOISE_SUPPRESSION`                                   | `int`    |      |
| `VRC_INPUT_MIC_ON_JOIN`                                             | `bool`   |      |
| `VRC_INPUT_MOBILE_INTERACTION_MODE`                                 | `int`    |      |
| `VRC_INPUT_MOUSE`                                                   | `int`    |      |
| `VRC_INPUT_OPENXR`                                                  | `int`    |      |
| `VRC_INPUT_OSC`                                                     | `int`    |      |
| `VRC_INPUT_PERSONAL_SPACE`                                          | `int`    |      |
| `VRC_INPUT_QUEST_HANDS`                                             | `int`    |      |
| `VRC_INPUT_SELECTED_SAFETY_RANK`                                    | `json`   |      |
| `VRC_INPUT_SHOW_TOOLTIPS`                                           | `int`    |      |
| `VRC_INPUT_STEAMVR2`                                                | `int`    |      |
| `VRC_INPUT_TALK_DEFAULT_ON`                                         | `int`    |      |
| `VRC_INPUT_TALK_TOGGLE`                                             | `int`    |      |
| `VRC_INPUT_THIRD_PERSON_ROTATION`                                   | `int`    |      |
| `VRC_INPUT_TOUCH`                                                   | `int`    |      |
| `VRC_INPUT_TOUCHSCREEN`                                             | `int`    |      |
| `VRC_INPUT_VALVE_INDEX`                                             | `int`    |      |
| `VRC_INPUT_VIVE`                                                    | `int`    |      |
| `VRC_INPUT_VIVE_ADVANCED`                                           | `int`    |      |
| `VRC_INPUT_VOICE_PRIORITIZATION`                                    | `int`    |      |
| `VRC_INPUT_WAVE`                                                    | `int`    |      |
| `VRC_INTERACT_HAPTICS_ENABLED`                                      | `bool`   |      |
| `VRC_INVERT_CONTROLLER_VERTICAL_LOOK`                               | `bool`   |      |
| `VRC_INVERTED_MOUSE`                                                | `bool`   |      |
| `VRC_IS_BOOPING_ENABLED`                                            | `bool`   |      |
| `VRC_LANDSCAPE_FOV`                                                 | `float`  |      |
| `VRC_LIMIT_DYNAMIC_BONE_USAGE`                                      | `bool`   |      |
| `VRC_LIMIT_PARTICLE_SYSTEMS`                                        | `bool`   |      |
| `VRC_MAIN_MENU_MOVEMENT_LOCKED`                                     | `bool`   |      |
| `VRC_MIC_ICON_FADE_OUT`                                             | `bool`   |      |
| `VRC_MIC_ICON_VISIBILITY`                                           | `bool`   |      |
| `VRC_MIC_TOGGLE_VOLUME`                                             | `float`  |      |
| `VRC_MIRROR_RESOLUTION`                                             | `int`    |      |
| `VRC_MM_FREE_PLACEMENT_ENABLED`                                     | `bool`   |      |
| `VRC_MOBILE_AUTO_HOLD_ENABLED`                                      | `bool`   |      |
| `VRC_MOBILE_AUTO_WALK_ENABLED`                                      | `bool`   |      |
| `VRC_MOBILE_DPI_SCALING`                                            | `float`  |      |
| `VRC_MOBILE_NOTIFICATIONS_SERVICE_ENABLED`                          | `bool`   |      |
| `VRC_MOBILE_PERFORMANCE_SECONDARY_UI_ENABLED`                       | `bool`   |      |
| `VRC_MOBILE_QUICK_SELECT_ENABLED`                                   | `bool`   |      |
| `VRC_MOBILE_TARGET_FRAME_RATE`                                      | `int`    |      |
| `VRC_MOUSE_SENSITIVITY`                                             | `float`  |      |
| `VRC_NAMEPLATE_FALLBACK_ICON_VISIBLE`                               | `bool`   |      |
| `VRC_NAMEPLATE_MODE`                                                | `int`    |      |
| `VRC_NAMEPLATE_OPACITY`                                             | `float`  |      |
| `VRC_NAMEPLATE_QUICK_MENU_INFO`                                     | `int`    |      |
| `VRC_NAMEPLATE_SCALE`                                               | `float`  |      |
| `VRC_NAMEPLATE_SCALE_V2`                                            | `int`    |      |
| `VRC_NAMEPLATE_STATUS_MODE`                                         | `int`    |      |
| `VRC_ONLY_SHOW_FRIEND_JOIN_LEAVE_PORTAL_NOTIFICATIONS`              | `bool`   |      |
| `VRC_PEDESTAL_SHARING_VISIBILITY`                                   | `bool`   |      |
| `VRC_PLAY_NOTIFICATION_AUDIO`                                       | `bool`   |      |
| `VRC_PLAYER_GESTURE_TOGGLE`                                         | `bool`   |      |
| `VRC_PORTAL_MODE`                                                   | `int`    |      |
| `VRC_PORTAL_MODE_V2`                                                | `int`    |      |
| `VRC_PORTRAIT_FOV`                                                  | `float`  |      |
| `VRC_PREFERRED_TIMEZONE`                                            | `string` |      |
| `VRC_PREFERRED_TIMEZONE_2`                                          | `string` |      |
| `VRC_PRINT_VISIBILITY`                                              | `bool`   |      |
| `VRC_PRIORITIZE_DOWNLOAD_DISTANCE`                                  | `float`  |      |
| `VRC_PRIORITIZE_FRIEND_DOWNLOADS`                                   | `bool`   |      |
| `VRC_PRIORITIZE_MANUAL_DOWNLOADS`                                   | `bool`   |      |
| `VRC_REDUCE_ANIMATIONS`                                             | `bool`   |      |
| `VRC_SAFETY_LEVEL`                                                  | `int`    |      |
| `VRC_SCREEN_BRIGHTNESS`                                             | `float`  |      |
| `VRC_SCREEN_CONTRAST`                                               | `float`  |      |
| `VRC_SEARCH_SAVED_SEARCHES`                                         | `string` |      |
| `VRC_SELECTED_NETWORK_REGION`                                       | `int`    |      |
| `VRC_SHOW_COMMUNITY_LAB_WORLDS_IN_SEARCH`                           | `bool`   |      |
| `VRC_SHOW_COMMUNITY_LABS`                                           | `bool`   |      |
| `VRC_SHOW_COMPATIBILITY_WARNINGS`                                   | `bool`   |      |
| `VRC_SHOW_FRIEND_REQUESTS`                                          | `bool`   |      |
| `VRC_SHOW_GO_BUTTON_IN_LOAD`                                        | `bool`   |      |
| `VRC_SHOW_GROUP_BADGE_TO_OTHERS`                                    | `bool`   |      |
| `VRC_SHOW_GROUP_BADGES`                                             | `bool`   |      |
| `VRC_SHOW_INVITES_NOTIFICATION`                                     | `bool`   |      |
| `VRC_SHOW_JOIN_NOTIFICATIONS`                                       | `bool`   |      |
| `VRC_SHOW_LEAVE_NOTIFICATIONS`                                      | `bool`   |      |
| `VRC_SHOW_PORTAL_NOTIFICATIONS`                                     | `bool`   |      |
| `VRC_SHOW_SOCIAL_RANK`                                              | `bool`   |      |
| `VRC_SKIP_GO_BUTTON_IN_LOAD`                                        | `bool`   |      |
| `VRC_SLIDER_SNAPPING`                                               | `bool`   |      |
| `VRC_STREAMER_MODE_ENABLED`                                         | `bool`   |      |
| `VRC_SUPPRESS_REMOTE_SHUTTER_SOUNDS`                                | `bool`   |      |
| `VRC_TIME_FORMAT_MODE`                                              | `int`    |      |
| `VRC_TOGGLE_HUD`                                                    | `bool`   |      |
| `VRC_TOUCH_AUTO_ROTATE_SPEED`                                       | `float`  |      |
| `VRC_TOUCH_SENSITIVITY`                                             | `float`  |      |
| `VRC_TRACKING_CAN_SHOW_SELFIE_FACE_TRACKING_POPUP`                  | `bool`   |      |
| `VRC_TRACKING_CAN_SHOW_TRY_WEBCAM_FACE_TRACKING_BUTTON`             | `bool`   |      |
| `VRC_TRACKING_DISABLE_EYELIDTRACKING`                               | `bool`   |      |
| `VRC_TRACKING_DISABLE_EYELOOKTRACKING`                              | `bool`   |      |
| `VRC_TRACKING_DISABLE_EYETRACKING_ON_MUTE`                          | `bool`   |      |
| `VRC_TRACKING_DISPLAY_EYETRACKING_DEBUG`                            | `bool`   |      |
| `VRC_TRACKING_ENABLE_SELFIE_FACE_TRACKING`                          | `bool`   |      |
| `VRC_TRACKING_ENABLE_SELFIE_FACE_TRACKING_AUTO_QUALITY`             | `bool`   |      |
| `VRC_TRACKING_ENABLE_SELFIE_HAND_TRACKING`                          | `bool`   |      |
| `VRC_TRACKING_FORCE_EYETRACKING_RAYCAST`                            | `bool`   |      |
| `VRC_TRACKING_FORCE_EYETRACKING_RAYCAST_DESKTOP`                    | `bool`   |      |
| `VRC_TRACKING_FORCE_EYETRACKING_RAYCAST_DESKTOP2`                   | `bool`   |      |
| `VRC_TRACKING_GRACEFUL_QUIT`                                        | `int`    |      |
| `VRC_TRACKING_NUM_TIMES_DISABLED_SELFIE_FACE_TRACKING`              | `int`    |      |
| `VRC_TRACKING_NUM_TIMES_ENABLED_SELFIE_FACE_TRACKING`               | `int`    |      |
| `VRC_TRACKING_NUM_TIMES_REFUSED_SELFIE_FACE_TRACKING_POPUP`         | `int`    |      |
| `VRC_TRACKING_NUM_TIMES_REFUSED_SELFIE_FACE_TRACKING_POPUP_VRC_PLUS`| `int`    |      |
| `VRC_TRACKING_NUM_TIMES_SELFIE_FACE_TRACKING_POPUP_VRC_PLUS_CLICKED`| `int`    |      |
| `VRC_TRACKING_SELFIE_FACE_TRACKING_QUALITY_LEVEL`                   | `int`    |      |
| `VRC_TRACKING_SELFIE_FACE_TRACKING_RECENTER_SPEED`                  | `float`  |      |
| `VRC_TRACKING_SEND_VR_SYSTEM_HEAD_AND_WRIST_OSC_DATA`               | `bool`   |      |
| `VRC_TRACKING_SHOULD_SHOW_OSC_TRACKING_DATA_REMINDER`               | `bool`   |      |
| `VRC_TRACKING_TRACKER_PREDICTION`                                   | `float`  |      |
| `VRC_UI_HAPTICS_ENABLED`                                            | `bool`   |      |
| `VRC_UI_HEADER_CLICK_SCROLL_RESET_ENABLED`                          | `bool`   |      |
| `VRC_USE_COLOR_FILTER`                                              | `bool`   |      |
| `VRC_USE_GENERIC_INSTANCE_NAMES`                                    | `bool`   |      |
| `VRC_USE_OUTLINE_MIC_ICON`                                          | `bool`   |      |
| `VRC_USE_PIXEL_SHIFTING_HUD`                                        | `bool`   |      |
| `VRC_VR_AUTO_HOLD_ENABLED`                                          | `bool`   |      |
| `VRC_WEBCAM_DEVICE_NAME`                                            | `string` |      |
| `VRC_WING_PERSISTENCE_ENABLED`                                      | `bool`   |      |
| `VRC_WORLD_TOOLTIP_MODE`                                            | `int`    |      |
| *Wing Menu UI State* | | |
| `Wing_{Left\|Right}_Avatars_Category`                               | `string` |      |
| `Wing_{Left\|Right}_Avatars_SortBy`                                 | `string` |      |
| `Wing_{Left\|Right}_Emojis_Category`                                | `string` |      |
| `Wing_{Left\|Right}_Friends_Category`                               | `string` |      |
| `Wing_{Left\|Right}_Friends_SortBy`                                 | `string` |      |
| `Wing_{Left\|Right}_Groups_Selected_Group`                          | `string` | `<GroupID>` |
| `Wing_{Left\|Right}_Worlds_Category`                                | `string` |      |
| `Wing_{Left\|Right}_Worlds_SortBy`                                  | `string` |      |

### Particularly Useful Entries

Here is a shortened list of (subjectively) more useful entries.

| Name                                    | Type     | Info |
| --------------------------------------- | -------- | ---- |
| `AVATAR_WORN_HISTORY_{UserID}`          | `json`   | `<History(AvatarID)>` |
| `AVProMovieCapture-LastSavedFile`       | `string` | path to temporary video file |
| `COLOR_PALETTES_{UserID}`               | `json`   | `<1[ <ColorPalette>,  ... ]>` The entry always seems to begin with a `1` character, the significance of which is unknown. |
| `COLOR_PALETTES_CURRENT_{UserID}`       | `json`   | `<ColorPalette>` |
| `{UserID}_DroneControllerSettings`      | `json`   | `<DroneControllerSettings>` |
| `{UserID}_DroneFlightPresetValues`      | `json`   | `<DroneFlightPresetValues>` |
| `FRIEND_LAST_VISIT_HISTORY_{UserID}`    | `json`   | `<History(UserID)>` |
| `HUD_SETTINGS`                          | `json`   | `<HudSettings>` |
| `I2 Language`                           | `string` |      |
| `INSTANCE_RENAME_HISTORY_{UserID}`      | `json`   | `<History(string)>` |
| `LocationContext`                       | `int`    | 0-2  |
| `LocationContext_World`                 | `string` | `<WorldID>|<string>`, where the last string is the world name |
| `LOD_QUALITY`                           | `int`    |      |
| `LOGGING_ENABLED`                       | `bool`   |      |
| `PlayerHeight`                          | `float`  | meters |
| `Preload_Worlds_HISTORY_{UserID}`       | `json`   | `<History(WorldID)>` |
| `RECENTLY_VISITED_HISTORY_{UserID}`     | `json`   | `<History(UserID)>` |
| `SavedWorldSearches{UserID}`            | `json`   | `{ "Searches": [ ... ] }` |
| `SeatedPlayEnabled`                     | `bool`   |      |
| `{UserID}_UI.Emojis.CustomGroup{0-2}`   | `json`   | `<CustomGroup(ItemID)>` |
| `{UserID}_UI.Props.CustomGroup{0}`      | `json`   | `<CustomGroup(ItemID)>` |
| `{UserID}_UI.RecentlyUsedEmojis`        | `json`   | `<CustomGroup(ItemID)>` |
| `{UserID}_UI.RecentlyUsedStickers`      | `json`   | `<CustomGroup(ItemID)>` |
| `{UserID}_UI.Stickers.CustomGroup{0-2}` | `json`   | `<CustomGroup(ItemID)>` |
| `VRC_AFK_ENABLED`                       | `bool`   |      |
| `VRC_CURRENT_LANGUAGE`                  | `string` |      |
| `VRC_DEFAULT_DRONE_SKIN_PALETTE`        | `json`   | `{ "Current": "#2a4bfc,#38bbff,#0076ff", "Default": "#2a4bfc,#38bbff,#0076ff" }` |
| `VRC_EARMUFF_MODE`                      | `bool`   |      |
| `VRC_GROUP_ON_NAMEPLATE`                | `string` | `<GroupID>` |
| `VRC_GROUP_ORDER_{UserID}`              | `json`   | `[ <GroupID> ]` |
| `VRC_IK_AVATAR_MEASUREMENT_TYPE`        | `int`    |      |
| `VRC_IK_DISABLE_SHOULDER_TRACKING`      | `bool`   |      |
| `VRC_IK_FBT_LOCOMOTION`                 | `bool`   |      |
| `VRC_IK_FBT_SPINE_MODE`                 | `int`    |      |
| `VRC_IK_FREEZE_TRACKING_ON_DISCONNECT`  | `bool`   |      |
| `VRC_IK_HEIGHT_RATIO`                   | `float`  |      |
| `VRC_IK_KNEE_ANGLE`                     | `float`  |      |
| `VRC_IK_SHOULDER_WIDTH_COMPENSATION`    | `bool`   |      |
| `VRC_IK_WRIST_ANGLE`                    | `float`  |      |
| `VRC_INPUT_MIC_DEVICE_NAME`             | `string` |      |
| `VRC_INPUT_MIC_DEVICE_NAME_Desktop`     | `string` |      |
| `VRC_INPUT_MIC_DEVICE_NAME_VR`          | `string` |      |
| `VRC_INPUT_MIC_ENABLED`                 | `bool`   |      |
| `VRC_SAFETY_LEVEL`                      | `int`    |      |
| `VRC_WEBCAM_DEVICE_NAME`                | `string` |      |

### Schema and Format Definitions

#### Values & Interpolation

`*`: Anything
`X|Y`: Either X or Y
`M-N`: A value from M through N, inclusive

#### IDs

`UserID`: The ID of a user such as `usr_c1644b5b-3ca4-45b4-97c6-a2a0de70d469`
`GroupID`: The ID of a group such as `grp_7ccb6ca3-cd36-4dab-9ab1-7bcf08d794e4`
`WorldID`: The ID of a world such as `wrld_5b89c79e-c340-4510-be1b-476e9fcdedcc`
`TransactionID`: the ID of a transaction such as `txn_7d3d25ec-663e-406e-96a3-e2c4fc0d8104`

#### JSON Objects

`History(Key)`: A mapping from ids to ISO `date-time`s such as `2026-01-02T03:04:05.0670890T`:
```json
{
    "<Key>": <date-time>,
    ...
}
```
`CustomGroup(Value)`: A named list of ids:
```json
{
    "name": <string>,
    "ids": [
        <Value>,
        ...
    ]
}
```
`ColorPalette`: An object describing custom UI colors, where `hex-color`is an RGB hex color such as `#69BB42`:
```json
{
    "name": <string>,
    "id": <string>,,
    "highlights": <hex-color>,
    "icons": <hex-color>,
    "buttons": <hex-color>,
    "backgrounds": <hex-color>,
    "text": <hex-color>,
    "subtext": <hex-color>
}
```
`DroneControllerSettings`: An object describing the current Drone controls:
```json
{
    "controllerType": <integer>,
    "keyboardAndMouseControllerType": <integer>,
    "droneScreenMode": <int>,
    "controllerInversion": <boolean>,
    "controllerDeadzone": <float>,
    "angleCenteredThrottleBehavior": <integer>,
    "acroCenteredThrottleBehavior": <integer>,
    "controllerKeyboardSensitivity": <float>,
    "controllerRelativeMouseMode": <boolean>,
    "controllerMouseSensitivity": <float>,
    "controllerMouseRelativeRate": <float>,
    "controllerMouseRelativeCurve": <float>,
    "controllerMousePowerCurve": <float>,
    "fpvScreenSize": <float>,
    "fpvScreenDimming": <float>,
    "audioFeedbackVolume": <float>
}
```
`DroneFlightPresetValues`: An object describing the current Drone controls:
```json
{
    "SelectedPreset": <integer>,
    "Presets": {
        "<integer>": {
            "cameraTilt": <float>,
            "maxAngle": <float>,
            "expoPitch": <float>,
            "expoRoll": <float>,
            "expoYaw": <float>,
            "expoThrottle": <float>,
            "centerSensitivityPitch": <float>,
            "centerSensitivityRoll": <float>,
            "centerSensitivityYaw": <float>,
            "centerSensitivityThrottle": <float>,
            "maxRatePitch": <float>,
            "maxRateRoll": <float>,
            "maxRateYaw": <float>,
            "maxRateThrottle": <float>,
            "modeChangeSmoothingTime": <float>,
            "mass": <float>,
            "drag": <float>,
            "crossSectionArea": <float>,
            "motorPower": <float>
        },
        ...
    }
}
```
`HudSettings`: An object describing what is visible on the HUD:
```json
{
    "Mode": <integer>,
    "Anchor": <integer>,
    "ShowJoinNotifications": <boolean>,
    "ShowLeaveNotifications": <boolean>,
    "ShowPortalNotifications": <boolean>,
    "OnlyShowFriendJoinLeaveAndPortalNotifications": <boolean>,
    "ShowInvites": <boolean>,
    "ShowFriendRequests": <boolean>,
    "Opacity": <float>,
    "MicOpacity": <float>,
    "MicToggleVolume": <float>,
    "PlayNotificationAudio": <boolean>
}
```
