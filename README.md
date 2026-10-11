# Core Player

A frame-by-frame video player for animators, with a Maya-style timeline, smooth scrubbing, and two-way frame sync with Maya and Blender. Windows desktop app (Electron).

> Status: early version. Built and used on Windows. Not extensively tested on all video formats or software versions.

## Features

- **Frame-accurate stepping and scrubbing** — arrow keys, timeline drag, or drag on the video itself.
- **Auto FPS detection** (manual override in the fps menu).
- **In/Out range** — orange buttons, range slider, or number fields. Playback and the timeline stay inside the range.
- **Bookmarks**, loop toggle (red = on), speed 0.1x–2x, audio on/off.
- **Optimize** (bundled ffmpeg) — makes every frame a keyframe so scrubbing in both directions is smooth:
  - *Optimize All* — whole video, saved next to the original as `name_optimized.mp4`.
  - *Optimize Cropped* — only the In/Out range, saved as `name_optimized_f<first>-<last>_<fps>fps.mp4`. Original frame numbers are kept.
  - *Normal* — back to the original file. Generated files are reused next time.
- **Start frame** setting (File menu): 0, 1, 1001, anything.
- **Maya / Blender sync** — all connected programs follow each other.

## Install

Download `Core Player Setup x.y.z.exe` from Releases and run it. ffmpeg is bundled, nothing else to install. The installer is unsigned, so Windows SmartScreen may warn ("More info" → "Run anyway").

## Build from source

Requires [Node.js](https://nodejs.org) LTS.

```
npm install
npm start          # run
npm run build      # installer in dist/
```

## Shortcuts

| Key | Action |
| --- | --- |
| Left / Right | Previous / next frame (Shift = 10 frames) |
| Space | Play / pause |
| Home / End | Go to range start / end |
| I / O | Set In / Out at current frame |
| B | Add / remove bookmark |
| L | Loop on/off |
| Up / Down | Playback speed |
| Ctrl+O | Open video |

## Maya / Blender sync

Core Player listens on `127.0.0.1:45821` (local machine only). The scripts in `dcc/` connect to it; whichever program changes the frame, the others follow.

- **Maya:** paste `dcc/maya_core_player.py` into the Script Editor (Python tab) and run it. Call `core_player_stop()` to disconnect.
- **Blender:** Edit → Preferences → Add-ons → Install, select `dcc/blender_core_player.py`, enable it.

The dot on the **Sync** button is green when connected, orange if the fps of the video and the program differ. Click it to turn sync on/off. Set **File → Start frame** to match your scene's first frame.

Protocol: one JSON object per line, e.g. `{"t":"frame","f":120,"fps":24}`. Other tools (Unreal, etc.) can be added by writing a small client that speaks the same messages.

## Third-party software

ffmpeg is bundled through the [`ffmpeg-static`](https://www.npmjs.com/package/ffmpeg-static) package. The ffmpeg build it ships is licensed under the GPL (it includes x264). If you redistribute Core Player, check the license terms of ffmpeg and include the required notices.

## License

Add your own license here (for example MIT for the app code). This choice is yours.
