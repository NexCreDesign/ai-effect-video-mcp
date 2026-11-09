# How the AI checks results

[한국어](checking-video-and-audio.ko.md)

The AI cannot play a video or hear sound. It can still check your edit quite accurately. This page explains what the AI can see and how to ask.

## What the AI can see

| To check | What the AI gets |
|---|---|
| The current picture | One frame of the result, as a picture |
| Motion, fades, scene changes | Several pictures spread over a stretch (up to 16) |
| Small text or details | A part of the picture, enlarged |
| Whether there is sound, loudness, fades | Loudness numbers for 20 parts of a stretch, and a waveform picture |
| The video file itself | An mp4 file of the stretch and where it is (for you to play) |

## Try asking

> Check 1:20 to 1:40. Tell me when the text appears, whether the fade looks natural, and whether the sound drops out anywhere.

> Zoom in on the text in the top right around 3 seconds and check it isn't cut off.

> Check that the sound gets quieter over the last 5 seconds.

> Make 10 to 15 seconds into an mp4. I'll watch it myself.

The AI usually checks in this order:

1. Turns the times you gave into exact frame numbers.
2. Scans the stretch with several small pictures.
3. Looks again, more closely, only where something seems wrong.
4. Enlarges a part if needed.
5. Checks the sound with loudness numbers.

## Limits

- **It doesn't know what the sound is.** The AI measures loudness only. It cannot tell what is said or played.
- **Timing can be slightly off.** Sound is measured in 20 parts, so over a long stretch the moment something starts can be off by 0.1 to 0.2 seconds. For an exact answer, say "check just that part again, more narrowly."
- **Pictures fill up the conversation.** Each picture uses a lot of the conversation's space. Ask about a specific stretch rather than a whole long video.
- **Files are deleted after a day.** If you don't say where to save, check files go to a temporary folder and are deleted after a day. To keep one, say where, for example "make it as D:\check\a.mp4".

## Advanced

### Reading the sound numbers

- `peak_db`, `rms_db`: loudness in dB. 0 is the loudest; `-120` means silence.
- `silent_percent`: the share of the stretch that is nearly inaudible (below -60 dB)
- `levels`: the loudness of each of 20 parts. In a fade-out the numbers keep falling.
- `clipped_samples`: above 0 means the sound is so loud that it distorts.

### Sending file contents into the conversation (`embed`)

There is an option to put the mp4 or WAV contents directly into the conversation, but most AI apps cannot handle it. Some, such as Codex, turn it into text, which fills the conversation at once (a 2-second video became about a megabyte of text). The AI doesn't use it by default; don't ask for it.

### Automatic checks over long stretches (with ffmpeg)

If ffmpeg is installed on your PC and your AI can run commands (Claude Code, Codex and others), it can find scene changes, black frames and silence in an mp4 the editor made. ffmpeg is not included with the editor.

```
ffmpeg -hide_banner -i clip.mp4 -vf "select='gt(scene,0.3)',metadata=print:file=-" -an -f null -
ffmpeg -hide_banner -i clip.mp4 -vf blackdetect=d=0.1:pix_th=0.1 -an -f null -
ffmpeg -hide_banner -i clip.mp4 -af silencedetect=n=-50dB:d=0.5 -vn -f null -
```

The times found count from the start of `clip.mp4`, so they must be shifted by where the clip starts in the edit. Just ask the AI to "check those places again in the editor."

### Sending video to other AI services

Some AI services accept video files directly, but the file then leaves your PC. Decide based on what the video contains.
