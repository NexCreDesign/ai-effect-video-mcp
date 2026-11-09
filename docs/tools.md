<!-- Generated from the app's tool definitions. Do not edit by hand. -->

# Tools

AI Effect Video 1.0.0, 23 tools. Frames are work frames counted from 0 unless a description says otherwise.

- [`get_status`](#get_status)
- [`get_project`](#get_project)
- [`get_objects_at`](#get_objects_at)
- [`get_subtitles`](#get_subtitles)
- [`measure_text`](#measure_text)
- [`look_range`](#look_range)
- [`get_audio`](#get_audio)
- [`get_clip`](#get_clip)
- [`frame_at_time`](#frame_at_time)
- [`list_fonts`](#list_fonts)
- [`list_recent_projects`](#list_recent_projects)
- [`look`](#look)
- [`edit_project`](#edit_project)
- [`history`](#history)
- [`open_media`](#open_media)
- [`close_effect`](#close_effect)
- [`save_project`](#save_project)
- [`probe_media`](#probe_media)
- [`playback`](#playback)
- [`set_view`](#set_view)
- [`export`](#export)
- [`export_status`](#export_status)
- [`cancel_export`](#cancel_export)

## get_status

App state: open project, number of sources, current frame, playing, unsaved changes, edition (free, trial or full), export running, recovery_pending.

*Read-only.*

No parameters.

## get_project

The whole project: source (the work video), sources (each joined source with its keep and work_span), effect videos and their cuts, objects and their events, audio, subtitle, export_range, export settings, and a revision string.

*Read-only.*

No parameters.

## get_objects_at

Objects on screen at a frame, with their actual shape, effect position and segment. Default is the current frame.

*Read-only.*

| Parameter | Type | Required | Description |
|---|---|---|---|
| `frame` | integer or string |  | Frame from 0, or "mm:ss:ff" |

## get_subtitles

Subtitle lines with their work frames (end is exclusive). start and end limit the range.

*Read-only.*

| Parameter | Type | Required | Description |
|---|---|---|---|
| `end` | integer |  |  |
| `start` | integer |  |  |

## measure_text

Size of the box a text would take, outline included. Use it to place text against an edge.

*Read-only.*

| Parameter | Type | Required | Description |
|---|---|---|---|
| `font` | string |  |  |
| `size` | integer |  |  |
| `text` | string | yes |  |

## look_range

See a stretch of the work video as JPEGs: frames spread evenly over [start, end), first and last included, composed as they will be exported. The result lists the frame number of each image, in order.

*Read-only.*

| Parameter | Type | Required | Description |
|---|---|---|---|
| `count` | integer |  | How many images, default 8, at most 16. A shorter range gives every frame |
| `end` | any | yes | First frame after the range: a frame number or "mm:ss:ff" |
| `max_size` | integer |  | Longest side in pixels, default 512, at most 1024 |
| `start` | integer or string | yes | Frame from 0, or "mm:ss:ff" |

## get_audio

Make the mixed sound of a stretch of the work video into a 48 kHz stereo WAV file, with source on or off, effect audio and fades as they will be exported, and measure it. Returns path, peak_db, rms_db, silent_percent, clipped_samples, levels (20 parts, each with frame, rms_db and peak_db) and a waveform image. Without path it goes to a temporary folder, where files are deleted after a day. Fails with no_audio when there is no sound at all.

*Can overwrite files or drop work: the AI asks the user first.*

| Parameter | Type | Required | Description |
|---|---|---|---|
| `embed` | boolean |  | Also send the sound itself as audio content, up to 8 MB (about 40 seconds). Default false: only the path. Only when the user asks: clients that cannot play it receive it as a huge text |
| `end` | any | yes | First frame after the range: a frame number or "mm:ss:ff" |
| `overwrite` | boolean |  | Replace an existing file. Only after the user agreed; on file_exists, ask the user first |
| `path` | string |  | Output file; its folder must exist |
| `start` | integer or string | yes | Frame from 0, or "mm:ss:ff" |

## get_clip

Make a stretch of the work video into an H.264 mp4 file with sound, composed as it will be exported, and wait for it. Returns path, frames, bytes, duration_seconds (length of the clip) and elapsed_seconds (time it took to make). Shows no dialog and leaves the export settings alone. It must finish within 50 seconds; for anything longer use export. Without path it goes to a temporary folder, where files are deleted after a day.

*Can overwrite files or drop work: the AI asks the user first.*

| Parameter | Type | Required | Description |
|---|---|---|---|
| `embed` | boolean |  | Also send the video itself as a resource, up to 8 MB. Default false: only the path. Only when the user asks: most clients cannot show it and receive it as a huge text |
| `end` | any | yes | First frame after the range: a frame number or "mm:ss:ff" |
| `overwrite` | boolean |  | Replace an existing file. Only after the user agreed; on file_exists, ask the user first |
| `path` | string |  | Output file; its folder must exist |
| `start` | integer or string | yes | Frame from 0, or "mm:ss:ff" |

## frame_at_time

The work frame on screen at a time, in the work frame rate, without doing the arithmetic yourself. Frames are counted from the start of the work video, so 0 seconds is frame 0 of the first file. To cut a source, the same number counts from the start of that file.

*Read-only.*

| Parameter | Type | Required | Description |
|---|---|---|---|
| `time` | any | yes | Seconds as a number or string (83.5), or "mm:ss.mmm" or "hh:mm:ss.mmm". This is clock time, not the "mm:ss:ff" frame form, so three parts need the milliseconds: 00:03:00 is refused, 00:03:00.000 is three minutes |

## list_fonts

Font names usable in text shapes. An empty font name is the default font.

*Read-only.*

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string |  | Part of a name |

## list_recent_projects

Recently opened project files, newest first.

*Read-only.*

No parameters.

## look

See a frame as a JPEG. view "work" is the composed result as it will be exported, "source" the source frame before composing, "effect" the raw effect frame matched to that work frame. region shows one part at full resolution, in the pixels of that view; small parts are enlarged.

*Read-only.*

| Parameter | Type | Required | Description |
|---|---|---|---|
| `effect` | integer |  | Effect id for view "effect"; default is the one shown |
| `frame` | integer or string |  | Frame from 0, or "mm:ss:ff" |
| `max_size` | integer |  | Longest side in pixels, default 1024 |
| `region` | object |  | Object with `h`, `w`, `x`, `y`. |
| `view` | string: `work`, `source`, `effect` |  |  |

## edit_project

Change project items. All changes apply as one step or none do, shown on screen at once, one Ctrl+Z to undo.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `base_revision` | string |  | From get_project. If given and the user changed the project since, nothing applies and the error is conflict |
| `changes` | array | yes | See below. |

### `changes`

```text
Changes, applied in order. Each is an object with "op":
Objects: add_object {frame, shape, end_frame?, name?, effect?, blue_key?, effect_position?:[x,y]}; set_shape {id, frame, shape}; set_effect_position {id, frame, x, y}; end_object {id, frame} (gone from that frame); remove_event {id, frame}; delete_object {id}; rename_object {id, name}; set_hidden {id, hidden}; set_blue_key {id, on} (blue pixels show the source); set_object_effect {id, effect}; move_object {id, index}.
Shapes: {type:rect|ellipse, x, y, w, h}; {type:polygon, points:[[x,y],...]}; {type:text, x, y, text, size?, color?:[r,g,b], outline?:[r,g,b], font?}; {type:mosaic, x, y, w, h, block?}; {type:image, path, x?, y?, w?, h?}.
Tracks take effect:<id> or audio:<index>, and frames in file frames, not work frames: set_track_offset {offset}; split_track {file_frame}; set_segment {segment, start, end}; move_segment {segment, start}; remove_segment {segment}; reset_cuts {}.
Other: set_source_audio {on}; close_effect_audio {audio}; set_subtitle_offset {offset}; close_subtitle {}.
Sources: set_source_cut {source, start, end}; close_source {source}; move_source {source, index} (index from 1, 0 is the first source); set_source_fade {source, fade_in, fade_out} (work frames, 0 = none). For set_source_cut, start and end are the kept part [start, end), in work frames from the start of that file, end exclusive. Results give keep, adjusted, work_span and deleted_objects.
```

## history

Undo or redo one edit step. This is the only way to undo; do not undo by making other changes.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `action` | string: `undo`, `redo` | yes |  |

## open_media

Open a file on this PC by path. source and project replace the current work. source_append joins a video after the last source (never in the middle); it must have the size of the first source, the frame rate may differ. effect, effect_audio and subtitle add to it.

*Can overwrite files or drop work: the AI asks the user first.*

| Parameter | Type | Required | Description |
|---|---|---|---|
| `discard_changes` | boolean |  | Drop unsaved work. Only after the user agreed; on unsaved_changes, ask the user first |
| `kind` | string: `source`, `source_append`, `effect`, `effect_audio`, `subtitle`, `project` | yes |  |
| `offset` | integer |  | effect_audio only: work frame where the file starts |
| `path` | string | yes |  |
| `relink` | object |  | project only: old path to new path, for files that moved |

## close_effect

Close an effect video. Objects stay and show the source until they get another effect. Cannot be undone.

*Can overwrite files or drop work: the AI asks the user first.*

| Parameter | Type | Required | Description |
|---|---|---|---|
| `effect` | integer | yes |  |

## save_project

Save the project. Without path, saves to its current file. Fails with file_exists for another existing file: ask the user before passing overwrite.

*Can overwrite files or drop work: the AI asks the user first.*

| Parameter | Type | Required | Description |
|---|---|---|---|
| `overwrite` | boolean |  | Replace an existing file. Only after the user agreed; on file_exists, ask the user first |
| `path` | string |  |  |

## probe_media

Read a video file's size, frame rate, frame count and audio without opening it.

*Read-only.*

| Parameter | Type | Required | Description |
|---|---|---|---|
| `path` | string | yes |  |

## playback

Move the current frame or play and pause in the app.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `action` | string: `seek`, `play`, `pause` | yes |  |
| `frame` | integer or string |  | Frame from 0, or "mm:ss:ff" |

## set_view

Change what the app shows: effect shown in the effect viewer, selected object, or the effect region On/Off preview.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `effect_enabled` | boolean |  |  |
| `selected` | integer or null |  |  |
| `shown_effect` | integer |  |  |

## export

Start exporting to a video file. Returns at once; poll export_status. Fails with file_exists when the file exists: ask the user before passing overwrite.

*Can overwrite files or drop work: the AI asks the user first.*

| Parameter | Type | Required | Description |
|---|---|---|---|
| `codec` | string: `h264`, `h265`, `av1`, `vp9` |  |  |
| `encoder` | string: `auto`, `nvenc`, `amf`, `qsv`, `media_foundation`, `svt_av1`, `aom`, `rav1e`, `vpx` |  |  |
| `overwrite` | boolean |  | Replace an existing file. Only after the user agreed; on file_exists, ask the user first |
| `path` | string | yes |  |
| `quality` | integer |  | 1 to 100. |
| `smart_copy` | boolean |  | Copy parts without objects as they are, when the codec matches the source |
| `speed` | string: `fast`, `balanced`, `quality` |  |  |

## export_status

Export state: idle, running, finished (encoder, elapsed_seconds, smart_not_used) or failed (message, retry_encoder).

*Read-only.*

No parameters.

## cancel_export

Cancel the running export.

No parameters.

## Error codes

A failed call returns `code: message` as text with `isError`. Codes the app can return:

- `bad_request`: the request is not valid: unknown or missing arguments, wrong values
- `busy`: an export is running
- `clip_too_long`: the audio did not finish in time and was cancelled; use a shorter range
- `closed`: the app is closing
- `conflict`: the project changed since base_revision; read it again with get_project
- `connection_lost`
- `encoder_unavailable`: no H.264 encoder on this PC
- `export_failed`
- `failed`
- `file_exists`
- `file_not_found`
- `first_source`: the first source cannot be moved and nothing can move before it
- `internal`: the request was dropped
- `invalid_shape`: a polygon needs at least 3 distinct points and some area
- `missing_media`
- `no_audio`: there is no sound: no source has audio or it is switched off, and there is no effect audio
- `no_effect`: no effect video is open
- `no_effect_frame`
- `no_event`
- `no_font`
- `no_object`
- `no_segment`
- `no_source`: no source video is open; use open_media first
- `no_source_clip`
- `no_subtitle`: no subtitle is open
- `no_track`
- `not_active`
- `not_connected`
- `not_exporting`: no export is running
- `nothing_to_do`: there is no step to do that with
- `open_failed`
- `out_of_range`: a polygon point is outside the video
- `path_required`: the project has no file yet; give a path
- `render_failed`
- `timeout`: the app did not answer in time; a dialog may be open in it. The request was cancelled and did not run
- `unknown_tool`
- `unsaved_changes`: the project has unsaved changes; save_project, or pass discard_changes if the user agreed
