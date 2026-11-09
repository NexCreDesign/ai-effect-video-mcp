# Example requests to AI

[한국어](conversations.ko.md)

Say things like these to the AI. File paths and numbers are examples; change them to match your files. You can word requests any way you like.

Open **What the AI does** under an example to see the commands the AI actually sends to the editor. That's only for the curious; you don't need it. (The examples assume a video with 30 frames per second.)

## 1. Open videos and sync them

> Open D:\videos\a.mp4 as the source and D:\videos\b.mp4 as the effect, and delay the effect by 12 frames.

Opens the original and the effect video, and makes the effect start 12 frames later than the original.

<details>
<summary>What the AI does</summary>

```json
{ "tool": "open_media", "arguments": { "kind": "source", "path": "D:\\videos\\a.mp4" } }
{ "tool": "open_media", "arguments": { "kind": "effect", "path": "D:\\videos\\b.mp4" } }
{ "tool": "edit_project", "arguments": { "changes": [
  { "op": "set_track_offset", "effect": 1, "offset": 12 }
] } }
```

The second command returns the effect's number (1 here), which the AI uses to delay it.
</details>

## 2. Put text on screen

> Put "Hello" in the top right from 3 seconds to 5 seconds.

The AI measures the text first so it sits neatly against the top right edge.

<details>
<summary>What the AI does</summary>

```json
{ "tool": "measure_text", "arguments": { "text": "Hello", "size": 72 } }
{ "tool": "edit_project", "arguments": { "changes": [
  { "op": "add_object", "frame": "00:03:00", "end_frame": "00:05:00",
    "shape": { "type": "text", "x": 1640, "y": 40, "text": "Hello", "size": 72 } }
] } }
```

`"00:03:00"` is minutes:seconds:frames, so 3 seconds.
</details>

## 3. Add subtitles

> Add D:\videos\c.srt and make it half a second earlier.

<details>
<summary>What the AI does</summary>

```json
{ "tool": "open_media", "arguments": { "kind": "subtitle", "path": "D:\\videos\\c.srt" } }
{ "tool": "edit_project", "arguments": { "changes": [
  { "op": "set_subtitle_offset", "offset": -15 }
] } }
```

Half a second is 15 frames in a 30-frame video.
</details>

## 4. Join videos, cut and fade

> Add D:\videos\d.mp4 at the end, cut off its first 2 seconds, and fade it in over one second.

A new video always goes at the end. To change the order, say "move the second video to third place" (the first video cannot be moved).

<details>
<summary>What the AI does</summary>

```json
{ "tool": "open_media", "arguments": { "kind": "source_append", "path": "D:\\videos\\d.mp4" } }
{ "tool": "edit_project", "arguments": { "changes": [
  { "op": "set_source_cut", "source": 2, "start": 60, "end": 900 },
  { "op": "set_source_fade", "source": 2, "fade_in": 30, "fade_out": 0 }
] } }
```

Cuts count frames from the start of that file. The AI reads the file's length first to set `end` (the end of the part to keep).
</details>

## 5. Check the result

> Show me the screen now.

> Check that the fade at 1:20 looks right.

The AI looks at the stretch as several pictures and checks the sound level too. See [How the AI checks results](../docs/checking-video-and-audio.md).

<details>
<summary>What the AI does</summary>

```json
{ "tool": "look", "arguments": {} }
{ "tool": "frame_at_time", "arguments": { "time": "1:20" } }
{ "tool": "look_range", "arguments": { "start": 2400, "end": 2431, "count": 6, "max_size": 384 } }
{ "tool": "get_audio", "arguments": { "start": 2400, "end": 2431 } }
```
</details>

## 6. Undo

> Undo that.

Each request to the AI is undone in one step. Pressing Ctrl+Z in the editor does the same.

<details>
<summary>What the AI does</summary>

```json
{ "tool": "history", "arguments": { "action": "undo" } }
```
</details>

## 7. Save and export

> Save the project as D:\work\my.evproj.

> Export to D:\out\final.mp4 in H.265.

Exporting takes a while. The AI keeps an eye on it and tells you when it's done. If a file with that name exists, it asks before replacing it.

<details>
<summary>What the AI does</summary>

```json
{ "tool": "save_project", "arguments": { "path": "D:\\work\\my.evproj" } }
{ "tool": "export", "arguments": { "path": "D:\\out\\final.mp4", "codec": "h265" } }
{ "tool": "export_status", "arguments": {} }
```
</details>
