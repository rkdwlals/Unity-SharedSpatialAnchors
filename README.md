![Sample Banner](Documentation/Media/colocated-block-toss.gif 'Unity SSA Sample')

# Unity-SharedSpatialAnchors

This project demonstrates how to use the multiplayer-oriented APIs for spatial anchors
(`OVRSpatialAnchor` from the [Meta XR Core SDK]).

| Overview                                              |
|-------------------------------------------------------|
| [Supported Devices](#supported-devices)               |
| [In this sample](#in-this-sample)                     |
| [Setup Overview](#setup-overview)                     |
| [↪ Step-by-Step Instructions](Documentation/Setup.md) |
| [Additional Resources](#additional-resources)         |

## Supported Devices

At the time of writing, the following Meta headsets support sharing spatial anchors:
- Quest Pro
- Quest 2
- Quest 3
- Quest 3S

For the most part, shared spatial anchors do work with **PC Link**—however, the feature was designed mostly with
Standalone (APK) apps in mind, and you may find the utility of shared anchors intrinsically more limited in tethered
contexts.

## In this sample:

Generally, this sample covers a brief review of basic spatial anchor creation, deletion, saving, and loading.
> For a more thorough overview of singleplayer-oriented spatial anchors, refer to
> [Unity-StarterSamples > Spatial Anchors](https://developers.meta.com/horizon/documentation/unity/unity-sf-spatial-anchors).

Beyond that:

### Scene "**Sharing Anchors to Users** (PUN)"
- path: `Assets/Scenes/SharedSpatialAnchors.unity`

Shows how a networking layer—Photon Realtime | PUN2 in this example—can be used to:
- connect players
- share/load anchors among themselves by User ID (obtained from [Oculus.Platform.GetLoggedInUser()](https://developers.meta.com/horizon/documentation/unity/ps-presence#retrieve-information-about-the-current-user))
- use simple persistence with anchor IDs to load them between rooms and sessions
- align to a shared anchor so that non-anchored objects ("networked cubes") appear to be in the same location and
  orientation to all users

See: [SharedAnchor.cs](Assets/Scripts/SharedAnchor.cs), [SharedAnchorLoader.cs](Assets/Scripts/SharedAnchorLoader.cs), [PhotonAnchorManager.cs](Assets/Scripts/PhotonAnchorManager.cs), [AlignPlayer.cs](Assets/Scripts/AlignPlayer.cs)

### Scene "**Colocation & Group Sharing** (PUN-free)"
- path: `Assets/Scenes/ColocationSessionGroups.unity`

Shows how to pair newer variants of the sharing/loading APIs along with the `OVRColocationSession` API to:
- connect players anonymously using local area colocation (powered by bluetooth) and "advertising / discovering"
  sessions
- share to / load from the Guid generated by these shared sessions (called a "Group UUID")
- transmit immutable, custom data associated with these sessions to, for instance, synchronize world alignment between
  group peers
- use an arbitrary ("hardcoded") UUID for group sharing/loading, which can be useful for testing since you can skip any
  "connection" steps
  - _Note:_ By default, the hardcoded UUID is the same as the APK's `Application.buildGuid`, meaning users must be on
    the same build for it to work.

See: [ColoDiscoMan.cs](Assets/Scripts/ColoDiscoMan.cs), [ColoDiscoAnchor.cs](Assets/Scripts/ColoDiscoAnchor.cs)


### TIP: Finding key API calls in the code

As much as possible, we've taken the liberty of marking key API calls relevant to this sample's goals using C# code
comments.

You can easily find them by searching for the string "`KEY API CALL`" (or "`// KEY API CALL`") in your IDE / text editor.
- The comments are always situated exactly 1 line above the API callsite they pertain to.

Or, if you're comfortable with CLI, we highly recommend using the `git grep` command to efficiently perform this search:

```bash
git grep -nF '// KEY API CALL' -- '*.cs'
# or:
git grep -nFA1 '// KEY API CALL' -- '*.cs'
# or to just list files that match:
git grep -lF '// KEY API CALL'
```

- `-n` = show line Numbers
- `-F` = Fixed string search (disables regex)
- `-A1` = print 1 additional line After each match (showing you the API call itself)
- `-l` = List matching file paths instead of line contents



## Setup Overview

|                               |                                                                                                                                                         |
|:------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------|
| "Before You Begin"            | https://developers.meta.com/horizon/documentation/unity/unity-before-you-begin<br/>(use the below Unity version)                                        |
| Unity Editor Version          | 1. This project uses **2021.3.32f1**[^1]. Install this version for best results.<br/>2. Make sure to install all Android modules.                       |
| Get & open the sample project | e.g. `git clone git@github.com:oculus-samples/Unity-SharedSpatialAnchors.git`                                                                           |
| [dashboard.oculus.com]        | Register a new **dummy** app at [dashboard.oculus.com] and hook the App ID into Unity.<br/>This is only required for sharing anchors directly to users. |
| [dashboard.photonengine.com]  | Register your app with [Photon][dashboard.photonengine.com] and hook the app ID into Unity.<br/>Can skip if not sampling Photon-driven scenes/features. |
| Build and flash an APK        | There's not better way to test out this sample than to make a build and run it!                                                                         |
| Play around with it!          | Feel free to change whatever you wish in your local copy of the sample—experiment<br/>and discover new possibilities!                                   |
| [Meta Quest Developer Hub]    | This is __optional__ but recommended for all developers.<br/>It provides a convenient GUI for sideloading test APKs as well as other handy tools.       |

### [Step-by-Step Instructions](Documentation/Setup.md)

For detailed setup instructions, check out the [Setup doc](Documentation/Setup.md).

------------------------------------------------------------------------------------------------------------------------

## Additional Resources

#### [Glossary of Terms & Concepts](Documentation/Glossary.md)
- This glossary is a somewhat free-form index of relevant terms and concepts that you may find used in this project,
  as well as in the myriad of related projects and documentation.

#### [Spatial Anchors API (Unity)](https://developers.meta.com/horizon/documentation/unity/unity-spatial-anchors-persist-content#implementation)
- Covers the technical details and code snippets (namely **the C# side**) for the fundamental operations on (local)
  spatial anchors:
  - Creating
  - [Saving](Documentation/Glossary.md#saved-anchor)
    *(does not cover [serializing](Documentation/Glossary.md#serialized-anchor-locally-saved-anchor))*
  - Loading + Localizing + Binding
  - Erasing
  - [Destroying / "Hiding"](Documentation/Glossary.md#to-hide-an-anchor)

#### [*Shared* Spatial Anchors API (Unity)](https://developers.meta.com/horizon/documentation/unity/unity-shared-spatial-anchors)
- Covers much of the same material as this project, split similarly into 2 families of overloads:
  - Group-based sharing
  - User-based sharing

#### [Configuring Your Device for ADB](https://developers.meta.com/horizon/documentation/unity/ts-adb)

#### [Using Quest Link for Unity Development](https://developers.meta.com/horizon/documentation/unity/unity-link)
- SSAs are supported with Quest Link, however its usefulness for testing may be limited by the multiplayer aspect.
- For the majority of cases, we recommend testing this sample and your own SSA apps with built debug APKs.

#### [License](LICENSE)

This project, Unity-SharedSpatialAnchors, is licensed under the most-recent version of the
["LICENSE" file at the root of the repository](LICENSE).

Some scenes in this sample project use Photon Realtime to share anchor data and implement networked objects in
[colocated](Documentation/Glossary.md#colocated) multiplayer spaces.
- The Photon implementation packaged with this project is the free version of Photon Unity Networking (PUN v2+),
  which can also be found on [the Unity asset store](https://assetstore.unity.com/packages/tools/network/pun-2-free-119922).
- PUN2 is covered by the [Standard Unity Asset Store EULA].
  - In order to *use* this distro of Photon, however, you will need to create an account with Exit Games
    (see [Setup](Documentation/Setup.md#photon)), which requires that you agree to
    [additional EULA terms](https://dashboard.photonengine.com/account/licenseterms/).
- Other sublicenses declared by Photon[^2][^3] may apply.

#### [Code of Conduct](CODE_OF_CONDUCT.md)

#### [Contributing](CONTRIBUTING.md)

------------------------------------------------------------------------------------------------------------------------

[Meta XR Core SDK]: https://developers.meta.com/horizon/downloads/package/meta-xr-core-sdk
[Standard Unity Asset Store EULA]: https://unity.com/legal/as-terms
[Unity Hub]: https://unity.com/download
[dashboard.oculus.com]: https://dashboard.oculus.com
[dashboard.photonengine.com]: https://dashboard.photonengine.com
[Meta Quest Developer Hub]: https://developers.meta.com/horizon/documentation/unity/unity-quickstart-mqdh

[^1]: Paste this into your browser's URL bar: `unityhub://2021.3.32f1/3b9dae9532f5`
[^2]: https://doc.photonengine.com/docs/content/oss-pun.pdf
[^3]: https://doc.photonengine.com/docs/content/OSS-.NET_Client_SDKs.pdf

[//]: # (Sample App Architecture: https://developer.oculus.com/documentation/unity/unity-ssa-sf/)
[//]: # (Scene Sharing: https://developer.oculus.com/documentation/unity/unity-shared-scene-sample/)
[//]: # (Health & Safety: https://developer.oculus.com/resources/unity-ssa-hs-app/)
