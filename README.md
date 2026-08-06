# CineShot Setup LITE

**Free camera-move and trailer authoring inside the Unity 6 editor. Requires Cinemachine 3.**

Build a camera move with sliders, shape its timing on a key timeline, cut cameras into a sequence
with transitions and impact shake, bake it to an AnimationClip, then hand it to Unity's Recorder
for an MP4. No timeline wrangling, no keyframe fiddling.

Everything happens in **one dockable window**: `Tools > CineShot Setup LITE`.

---

## What LITE includes

**Every feature of the full version**: sliders, key timeline with ease control, per-key shake,
sequence strip with transitions and impact shake, live Game-View preview, bake to AnimationClip,
Recorder export, export and import, demo scenes. Nothing is watermarked or time-limited.

The only limits:

| | LITE | Full |
|---|---|---|
| Cameras you create with **Set Camera** | **2** | unlimited |
| Shots in a **sequence** | **2** | unlimited |

Two cameras and one cut are enough to learn the whole workflow end to end. The limit only counts
cameras that already carry a camera move, so existing Cinemachine cameras in your scene never
block the button. The full version is coming to the Unity Asset Store.

---

## Requirements

| | |
|---|---|
| **Unity** | 6.0 LTS (6000.0) or newer, one build for all of Unity 6 |
| **Cinemachine** | 3.x, **required** (`com.unity.cinemachine`) |
| **Unity Recorder** | optional, only for the MP4 export (`com.unity.recorder`) |
| **Render pipeline** | Built-in, URP and HDRP, **a demo scene ships for each** |
| **uGUI** | `com.unity.ugui`, used for the Fade-to-Black overlay (ships with every Unity template) |

> A material can only reference one shader, so a single scene cannot serve all three pipelines.
> The demo therefore ships three times, as `HDRP/`, `URP/` and `Built-In/`, with identical layout,
> camera moves and sequence. **Open the one matching your project and delete the other two folders.**

Cinemachine 3 is a hard dependency: the tool drives `CinemachineCamera` components and will not
run without it. The Unity Recorder is **not** required. Without it the Record step is disabled and
everything else (authoring, preview, baking) works normally.

## Installation

1. Download `CineShotSetupLITE.unitypackage` from the [latest release](../../releases/latest) and
   import it via `Assets > Import Package > Custom Package…`.
2. Open `Tools > CineShot Setup LITE`.
3. Optional, for recording: install **Unity Recorder** via
   `Window > Package Manager > Unity Registry > Recorder`, then configure one Movie recorder in
   `Window > General > Recorder`. CineShot never changes those settings, it only opens the window.

You may rename or move the folder anywhere under `Assets/`, but keep it together: the `Editor`
folder must keep its `.uxml` and `.uss` next to the assembly.

## Quick start

1. Press **Set Camera** to create a Cinemachine camera at the current Scene-View position. The
   tool then offers look-at targets: pick one from the list, or hover the object directly in the
   Scene View (it highlights green) and click it. The pivot comes from the object's mesh bounds.
2. Move the camera with the **CONTROLS** sliders (orbit, crane, pan, dolly zoom, roll and so on).
   The sliders are *relative tools*: they move the live camera away from its start pose, and the
   Game View follows along live.
3. Press **+ Add Key** to snapshot that pose as a key. The sliders reset to zero, so the next move
   starts from the pose you just captured.
4. Repeat. Shape timing in the **TIMELINE** card: drag key bars to change duration, click the ease
   buttons (Smooth, Linear, Slow, Fast), drag the ease flags, mute channels via the eye icon, add
   Shake/Noise per key.
5. **Step 1: Bake** writes an AnimationClip + Animator and activates the trailer camera. The status
   light next to the button turns green when the bake is up to date (red = not baked yet, yellow =
   changed since the last bake).
6. **Step 2: Open Recorder** opens Unity's Recorder window. Enter the frame count shown under the
   button as the end frame and record your MP4.

For a two-camera trailer: select both cameras, press **+ Add selected cameras** in the **SEQUENCE**
card, then click the green **+** between the two camera keys to insert a transition (Linear /
Ease In / Ease Out / Ease In-Out / Fade Black). Drag a transition key sideways to set its duration;
drag a camera key sideways to reorder the shots; `Del` removes the selected key; **Delete All**
clears the sequence in one undoable step.

## Demo scene

One demo per render pipeline, pick the folder that matches your project:

| Pipeline | Scene | Data file |
|---|---|---|
| **HDRP** | `HDRP/Scenes/DemoScene` | `HDRP/Scenes/DemoScene_CineShotData.json` |
| **URP** | `URP/Scenes/DemoScene` | `URP/Scenes/DemoScene_CineShotData.json` |
| **Built-in** | `Built-In/Scenes/DemoScene` | `Built-In/Scenes/DemoScene_CineShotData.json` |

All three are identical in layout, camera moves and sequence. Only materials, sky and lighting
differ. The other two folders can be deleted; their materials cannot resolve their shader in your
pipeline and would show up pink.

Open the scene, then in the tool window click **Data > Import…** and pick the matching
`DemoScene_CineShotData.json`. That loads two ready-made camera moves (a 120 degree orbit with a
slight crane, then a dolly push with a tele zoom) and a finished two-shot sequence joined by an
Ease-In blend. Press **Play Sequence** to watch it, or click either `DemoCam_*` in the Hierarchy to
inspect its keys.

Both demo cameras already carry a move, so the LITE camera limit is reached inside the demo scene.
That is intentional: the demo is there to be watched and taken apart. In a scene of your own you
start at zero and can create two cameras of your own.

## Where your data lives

Authored keys, sequences, the music selection and the output folder are stored **per project** in:

```
<ProjectRoot>/UserSettings/CineShotData.json
```

`UserSettings/` is a Unity convention for per-machine settings and is normally **excluded from
version control**. That means:

* Camera moves are **not** shared with teammates through git/Perforce by default.
* To share or back them up, use **Data > Export…**, or commit `CineShotData.json` deliberately.
* Cameras are referenced by `GlobalObjectId`, which includes the scene GUID. Duplicating a scene
  or turning a camera into a prefab creates a new id, and the tool treats it as a new camera.

Baked artifacts (AnimationClips, Controllers) go to the folder shown in the **Output** row, by
default `Assets/CineShot Setup LITE/Animations`. Recorded videos go wherever *your* Recorder preset
points.

## FAQ / Troubleshooting

**"The LITE version lets you author camera moves on up to 2 cameras."**
You already have two cameras with moves on them. Delete the keys of one of them, work in another
scene, or move up to the full version. Cameras without any CineShot move do not count.

**The Record step is greyed out.**
The Unity Recorder package is missing. Hover the button, the tooltip says so.

**I pressed Open Recorder and got a warning first.**
The bake status light was red or yellow: red means nothing has been baked yet, yellow means you
changed something since the last bake. Bake again, then record.

**Nothing was recorded, or I sat in Play Mode doing nothing.**
Recording is driven entirely by Unity's Recorder window, not by CineShot. Open
`Window > General > Recorder`, make sure exactly one recorder is enabled and valid, set a start and
end frame, and press Record there.

**I change a camera's Field of View and nothing happens.**
FOV is a *driven* value here. The baked Trailer Camera's FOV is controlled by its Animator, and the
Main Camera's FOV is controlled by the CinemachineBrain, so both are outputs and a manual edit is
overwritten every frame. To change the FOV, use the **Zoom FOV** slider in the CONTROLS card
(negative = zoom in) and bake, or set it on the *source* camera's Lens before baking. The demo
scene's second shot has a -6 degree Zoom FOV, which is why its baked FOV reads as roughly 56
rather than 60.

**"CinemachineFocusDistanceCompute.compute not found".**
HDRP auto-focus needs Cinemachine's HDRP sample. Import it via
`Package Manager > Cinemachine > Samples`. Without it, everything except auto-focus works.

**The window is empty.**
The `.uxml` and `.uss` must stay inside the `Editor` folder next to the assembly. If they were
moved apart, the window says so in the console.

**Shake preview only shows in the Game View.**
The preview shakes the camera itself; the Scene View watches that camera from the outside, so the
wobble is only visible through the camera, that is, in the Game View.

**A camera key in the sequence strip is grey.**
Its camera lives in a scene that is not loaded, or has no keys. Grey shots are skipped during
playback and baking. Click it and press `Del` to remove it.

**Cinemachine's camera frusta disappeared from my Scene View.**
That is CineShot, on purpose: while the window is open it hides Cinemachine's own frusta so they do
not fight the calm game frame it draws instead. The original gizmo settings are saved and put back
when you close the window.

## Known limitations

* One LookAt target per camera timeline (switch targets by cutting to a second camera).
* `Fade Black` is currently the only overlay FX transition.

## About this package

The editor assembly ships **precompiled** (`CineShot.Editor.dll`). This is a free edition of a
commercial tool, not an open-source project. The runtime components and the Recorder bridge ship as
source so they integrate cleanly with your own scenes and build setup.

## License

Free for personal **and** commercial projects, see [LICENSE.md](LICENSE.md). Redistribution or
resale of the tool itself is not permitted. Anything you create with it is entirely yours.

## Feedback

Bug reports and feature requests are welcome, please open an issue. Include your Unity version,
render pipeline, Cinemachine version, and whether the Unity Recorder is installed.
