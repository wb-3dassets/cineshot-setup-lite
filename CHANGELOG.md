# Changelog

All notable changes to **CineShot Setup LITE** are documented here.

## [1.0.0] - Initial release

Camera-move authoring for Unity 6, requires Cinemachine 3.

* **Slider authoring**: 11 relative motion channels (orbit H/V, crane, pan side/forward/up,
  dolly zoom, yaw, tilt, roll, zoom) with fine relative dragging, per-key pose snapshots.
* **Key timeline**: drag-reorder, duration drag, ease modes (Smooth / Linear / Slow / Fast),
  draggable ease-phase flags, per-channel mute (eye icon), per-key shake with windowing,
  continuity linter, key reverse, white playhead as a draggable time slider.
* **Sequence strip**: chain cameras into a trailer with cuts, pose blends (Linear/Ease), Fade
  Black, drag-reorder shots, draggable transition durations, per-transition impact shake
  (Start/End anchor with standstill hold), Delete All.
* **Music sync**: waveform display, draggable offset, sample-accurate playback sync that also
  holds in recorded videos.
* **Bake**: one click writes an AnimationClip + Animator onto the camera (or a whole trailer
  onto the dedicated trailer camera). Preview and baked clip match, FOV and fade are baked as
  curves. A red/yellow/green status light shows whether the bake is up to date.
* **Record**: Step 2 opens the Unity Recorder window (optional package) with your own recorder
  preset. The tool reports the exact trailer length and frame count to enter as the end frame.
  CineShot never modifies your recorder settings.
* **Scene tooling**: Set Camera from the Scene View, look-at target detection with mesh pivots,
  hover-picking directly in the Scene View with a green highlight, decouple/couple, game-frame
  overlay, path visualization.
* **Data export/import**: save all camera moves and the sequence to a `.json` for backup, version
  control or teammates, and merge it back by camera identity.
* **Demo scene**: one per render pipeline (`HDRP/`, `URP/`, `Built-In/`), each with two authored
  camera moves and a finished two-shot sequence joined by an Ease-In blend. Open the scene, then
  `Data > Import…` the JSON next to it and press Play Sequence.
* **Documentation**: `Documentation/CineShot Setup LITE - Documentation.rtf` with 22 numbered
  sections, plus this README.
* Per-project persistence in `UserSettings/CineShotData.json` (atomic writes, backup copy on
  corruption), full undo/redo for all tool edits.
* One build for all of Unity 6: the assembly is compiled on 6000.0 LTS and runs unchanged on
  6.0, 6.3 and 6.5.

### LITE edition

This free edition is feature complete. It is limited to **2 cameras** created via *Set Camera*
and **2 shots** per sequence. The camera limit counts only cameras that already carry a CineShot
move, so existing Cinemachine cameras in your scene never block the button. The full version
removes both limits.
