[![Discord - AudioLink Discord](https://img.shields.io/badge/Discord-AudioLink_Discord-7289da?logo=discord&logoColor=7289da)](https://discord.gg/d5wjNwZBR3) [![Website - AudioLink Website](https://img.shields.io/badge/Website-AudioLink_Website-7289da)](https://audiolink.dev/)

# AudioLink

## A repository of audio reactive prefabs for Unity, written in CSharp and HLSL, compatible with VRChat and ChilloutVR

AudioLink is a system that analyzes and processes in-world audio into many different highly reactive data streams and exposes the data to Scripts and Shaders. 

The per-frequency audio amplitude data is first read briefly into Udon using Unity's GetOutputData. It is then sent to the GPU for signal processing and buffered into a CustomRenderTexture. Then, the CustomRenderTexture is broadcast globally (called `_AudioTexture`) which can be picked up by shaders both in-world and across all avatars. 

### [Public example world](https://vrchat.com/home/launch?worldId=wrld_8554f998-d256-44b2-b16f-74aa32aac214)
### [Frequently Asked Questions](FAQ.md)
### [Documentation for shader creators](https://github.com/llealloo/vrc-udon-audio-link/tree/master/Docs)

## 1.0.0
### New features
- Added a new AudioLink Controller, with a completely revamped design. The old controller is still included, if you don't want to change. (Thanks to everyone who helped, including Pema, Lea, Teeh, Sacred, TechAnon and more)
- Added automatic gain adjustment. It's enabled by default, but you can disable it on the AudioLink controller. (Thanks, cnlohr)
- Added Async GPU Readback, you can toggle it on the AudioLink prefab. This means you can read data from AudioLink in Udon, without having to pay a heavy performance penalty! Note that this feature only works on PC. Quest (and other mobile platforms) will continue to use the old slow synchronous readbacks, which we advise against.
- Added [a new C# API](https://github.com/llealloo/vrc-udon-audio-link/blob/master/Packages/com.llealloo.audiolink/Runtime/Scripts/AudioLink.DataAPI.cs) for reading audio data in Udon, to better accomodate the new Async Readbacks.
- Added Media Playback States, which provide information about the currently playing track. ([See Docs](https://github.com/llealloo/vrc-udon-audio-link/tree/master/Docs)). (Thanks, fundale)
- Added Media States Udon API [VideoPlayer API](https://github.com/llealloo/vrc-udon-audio-link/tree/master/Docs/PlayerAPI.md), to control the aforementioned Playback States.  (Thanks, fundale)
- Added an AudioLink prefab for ChilloutVR.
### Changes
- We have switched to using [Semantic Versioning](https://semver.org/), marking this release as 1.0.0. This may break shaders written for very old versions of AudioLink. The version will now read as `1.00f` to AudioLink shaders.
- The "VRCAudioLink" namespace has been renamed to "AudioLink". This is a breaking change. We've added a new scripting define, `AUDIOLINK_V1`, which will be present for future versions of AudioLink, to aid with backwards compatability.
- Removed the dependency on the VPM package resolver.
### Improvements
- Made yt-dlp integration resolve URLs asynchronously.
- Improved the yt-dlp integration UI in a number of ways. (Thanks, rRazgriz)
- Improved the "Add Audiolink Prefab to Scene" button.
- Improved logging, with less meaningless warning spam.
- Cache the local player in AudioLink scripts for slightly improved performance. (Thanks, Nessie)
- Fixed null reference exceptions being fired when a player leaves the world. (Thanks, Nessie)
- Fixed an issue where theme colors would reset to the defaults on world start (https://github.com/llealloo/vrc-udon-audio-link/issues/281).

## Updating projects from version 2.8 or lower? (...first time setup? please see next section)
1. Before upgrading your project, **MAKE A BACKUP**! The latest version of AudioLink changes many things - better safe than sorry.
2. Take note of which AudioSource you are using to feed AudioLink, this reference may be lost during upgrade.
3. If you haven't ever used VRChat Creator Companion (VCC) with your project, follow the steps below. Otherwise, skip to step 4:
   - Download and install the [VRChat Creator Companion](https://vrchat.com/download/vcc), open it up.
   - Use the "Add" option in the "Projects" tab and follow the steps shown to add your project to the VCC.
   - Open the Projects tab, select your project, press the "Migrate" button and follow the steps shown.
4. Open the Projects tab and select your project.
5. On the right side, find the AudioLink package and add it. If it doesn't show up, make sure you have the "Curated" toggle enabled in the top-right drop-down.
6. In a file browser, **without Unity open**, navigate to your projects Assets folder and delete the "AudioLink" folder and "AudioLink.meta" file.
7. Open the Project in Unity.
8. You may be prompted by the AudioLink shader upgrader to upgrade old shaders. You should do so if your project uses any custom AudioLink-enabled shaders.
9. If you were using assets from the AudioLink example scene, you'll have to import it, as it isn't imported by default. To do so, use the "AudioLink -> Open AudioLink Example Scene" in top menu of the editor.
10. If you were using AudioReactiveObject or AudioReactiveLight
   components, you may need to manually re-enable the "Audio Data" under AudioLink "experimental" settings. This feature is now considered experimental until VRChat *maybe* gives us native asynchronous readback.
11. In scene(s) containing old versions of AudioLink:
   - Delete both AudioLink and AudioLinkController prefabs from the scene.
   - Re-add AudioLink and AudioLinkController to the scene by dragging the prefabs from the Packages/com.llealloo.audiolink/Runtime folder.
   - Click the "Link all sound reactive objects to this AudioLink" button on AudioLink inspector panel.
   - Drag the AudioSource you were using previously into the AudioLink audio source parameter.
      - NOTE: If you previously used AudioLinkInput, you are welcome to continue doing so, however now in 2.5+ AudioLink is much smarter about inputs. Try dragging it straight into the AudioLink / audio source parameter!

## Upgrading avatar projects
1. In a file browser, delete the "Assets/AudioLink" folder and the "AudioLink.meta" file.
2. Follow the "First time setup" steps for avatar projects described below.

## First time setup
Looking to test out an avatar? See the "For Avatar Testing" section. Otherwise, see the "For Worlds" section below. After installation, check the "Getting Started" section for some tips.

### For Worlds
1. Download and install the [VRChat Creator Companion](https://vrchat.com/download/vcc) (VCC), open it up.
2. Add your project to the VCC:
   - If you want to create a new project, use the "New" option in the "Projects" tab and follow the steps there.
   - If you want to use an existing project, use the "Add" option in the "Projects" tab and follow the steps there.
3. Open the Projects tab and select your project. If you have never used the VCC with the project, use the "Migrate" button to upgrade it.
4. On the right side, find the AudioLink package and add it. If it doesn't show up, make sure you have the "Curated" toggle enabled in the top-right drop-down.
5. At this point, the installation is done. To open your project, you can use the "Open Project" button in the VCC. If you want to view the example scene, use the "AudioLink -> Open AudioLink Example Scene" button in the top menu of the editor.

### For Avatar Testing
1. Download and install the [VRChat Creator Companion](https://vrchat.com/download/vcc) (VCC), open it up.
2. Add your project to the VCC:
   - If you want to create a new project, use the "New" option in the "Projects" tab and follow the steps there.
   - If you want to use an existing project, use the "Add" option in the "Projects" tab and follow the steps there.
3. Open the Projects tab and select your project. If you have never used the VCC with the project, use the "Migrate" button to upgrade it.
4. On the right side, find the AudioLink package and add it. If it doesn't show up, make sure you have the "Curated" toggle enabled in the top-right drop-down.
5. Use the "AudioLink/Add AudioLink Prefab to Scene" menu item.
6. Under AudioLinkAvatar/AudioLinkInput, add a music track to the AudioClip in the AudioSource.
   - If you need it louder, duplicate the AudioLinkInput object and increase the volume on that one. Make sure Not to adjust the volume on the main AudioLinkInput object - it needs to stay at 0.01.
7. Enter playmode to test your avatar.

### For non-VRChat uses (including CVR)
1. Download and Import the latest **UnityPackage** AudioLink Release at https://github.com/llealloo/vrc-udon-audio-link/releases.
2. Use the "AudioLink/Add AudioLink Prefab to Scene" menu item. It should work out of the box.

## Getting started
After installation, to use AudioLink:
1. Use the "AudioLink/Add AudioLink Prefab to Scene" menu item.
2. Click the "Link all sound reactive objects..." button to link everything up.

If you want to see an example of a scene with AudioLink set up, press the "AudioLink -> Open AudioLink Example Scene" in the top menu of the editor.

## Compatible tools / assets
- [Silent Cel Shading Shader](https://gitlab.com/s-ilent/SCSS) by Silent
- [Mochies Unity Shaders](https://github.com/MochiesCode/Mochies-Unity-Shaders/releases) by Mochie
- [Fire Lite](https://discord.gg/24W435s) by Rollthered
- [VR Stage Lighting](https://github.com/AcChosen/VR-Stage-Lighting) by AcChosen
- [Poiyomi Shader](https://poiyomi.com/) by Poiyomi
- [orels1 AudioLink Shader](https://github.com/orels1/orels1-AudioLink-Shader) by orels1
- [VRC Things](https://github.com/PiMaker/VRChatUnityThings) by \_pi\_
- [June Screen FX](https://luka.moe/june.html) by luka
- [ShaderForge-AudioLink](https://github.com/lethanan/ShaderForge-AudioLink) by lethanan
- [AudioLink-USharpVideo-Adapter](https://github.com/Blabzillaweasel/AudioLink-USharpVideo-Adapter/) by Blabz
- [ProTV](https://gitlab.com/techanon/protv) by ArchiTechAnon

## Thank you
- phosphenolic for the math wizardry, conceptual programming, debugging, design help and emotional support!!!
- cnlohr for the help with the new DFT spectrogram and helping to port AudioLink to 100% shader code
- lox9973 for autocorrelator functionality and the inspirational & tangential math help with signal processing
- Texelsaur for the AudioLinkMiniPlayer and support!
- Pema for the help with strengthening the codebase and inspiration!
- 3 for joining the AudioLink team, helping maintain the codebase, and being instrumental in getting version 0.3.0 out.
- Merlin for making UdonSharp and offering many many pointers along the way. Thank you Merlin!
- Orels1 for all of the great help with MaterialPropertyBlocks & shaders and the auto configurator script for easy AV3 local testing
- Xiexe for the help developing and testing
- Thryrallo for the help setting up local AV3 testing functionality
- CyanLaser for making CyanEmu
- Lyuma for helping in many ways and being super nice!
- ACIIL for the named texture check in AudioLink.cginc
- fuopy for being awesome and reflecting great vibes back into this project
- Colonel Cthulu for incepting the idea to make the audio data visible to avatars
- jackiepi for math wizardry, emotional support and inspiration
- Barry and OM3 for stoking my fire!
- Lamp for the awesome example music and inspiration. Follow them!! https://soundcloud.com/lampdx
- Shelter, Loner, Rizumu, and all of the other dance communities in VRChat for making this
- rrazgriz for coming up with and implementing yt-dlp support for editor testing
- LucHeart and DomNomNom for maintaing CVR forks of AudioLink, and letting us adopt their work