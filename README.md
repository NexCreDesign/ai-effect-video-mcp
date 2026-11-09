# AI Effect Video MCP

[한국어](README.ko.md)

**AI Effect Video** is a video editor for Windows. It lays parts of another video (the effect video) over your original video, adds text, mosaics and pictures, and exports the result as a new video.

This repository shows **how to let an AI such as Claude or ChatGPT (Codex) operate the editor for you**. Once connected, you just tell the AI what you want, and the changes appear in the editor right away. If you don't like a change, press Ctrl+Z or keep editing by hand.

> MCP (Model Context Protocol) is a standard way for an AI to use programs on your PC. You don't need to know more than that; just follow the steps below.

## What you can ask

- "Open a.mp4 as the source and b.mp4 as the effect, and delay the effect by 12 frames."
- "Put the text 'Hello' in the top right from 3 seconds to 5 seconds."
- "Add a one-second fade-in at the start of the second video and check that it looks right."
- "Export to D:\out\final.mp4."

More examples are in [Example requests](examples/conversations.md).

## What you need

- A Windows 11 (64-bit) PC
- One AI app: **Claude Desktop** (easiest), Claude Code, **Work** mode in the ChatGPT desktop app, Codex, or VS Code
  - It does not work on the ChatGPT website or in **Chat** mode of the ChatGPT desktop app.

## Step 1: Get the editor

1. Download `EffectVideo-1.0.0.zip` from the [Releases page](https://github.com/NexCreDesign/AIEffectVideo_release/releases/latest).
2. Unzip it to `C:\Tools` (or another folder you will keep). The zip already contains a folder named `EffectVideo-1.0.0`, so you get `C:\Tools\EffectVideo-1.0.0`, the folder this guide uses. There is nothing to install.
   - In the Windows **Extract All** window, enter only `C:\Tools` as the destination. If the destination ends with `EffectVideo-1.0.0`, you get that folder twice, one inside the other.
3. Double-click `effect_video.exe` in that folder to try it once.
   - If a blue "Windows protected your PC" window appears, click **More info**, then **Run anyway**. It appears because the program is not code-signed yet.

The free edition has every feature. Exported videos carry a small "AI Effect Video Free" mark in the corner.

> `EffectVideo-1.0.0-ffmpeg-source.zip` on the same page is the source code of the FFmpeg parts inside the editor, provided for license reasons. You don't need it.

## Step 2: Connect your AI app (Claude Desktop)

* If your Claude Desktop has a tool that can edit files on your PC, you can give it the address https://github.com/NexCreDesign/ai-effect-video-mcp and ask it to do this setup. If it can't, or the setup doesn't work, follow the steps below.

1. Open Claude Desktop and choose **Settings › Developer › Edit Config**. A folder with the file `claude_desktop_config.json` opens.
2. Open that file in Notepad, put in the following, and save it.

   ```json
   {
     "mcpServers": {
       "ai-effect-video": {
         "command": "C:\\Tools\\EffectVideo-1.0.0\\effectvideo-mcp.exe"
       }
     }
   }
   ```

   - Change the path to where `effectvideo-mcp.exe` really is. In this file every `\` must be written **twice**, as `\\`.
   - If the file already has `"mcpServers"`, add only the `"ai-effect-video": { ... }` part inside its braces `{ }`, separated by a comma.
3. **Quit Claude Desktop completely** and start it again. (If it stays in the system tray after closing the window, quit it from the tray icon.)

For other AI apps, see [Connecting other AI apps](examples/other-clients.md).

## Step 3: Check the connection

Say to the AI:

> Show me the AI Effect Video status.

If the editor was not running, it starts by itself, and the AI tells you its version and what is open. The editor's status bar at the bottom shows **AI connected**.

If it doesn't work, see [Troubleshooting](docs/troubleshooting.md).

## Good to know

- **Safety:** the AI is told to ask you before it throws away unsaved work or replaces an existing file.
- **Undo:** each request to the AI can be undone with one Ctrl+Z. You can also say "undo that".
- **Only on your PC:** only programs on this PC can connect to the editor. Nothing can connect from the internet.
- **Turning it off:** switch off **Settings › Allow AI connection (MCP)** in the editor. It is on by default.
- **What the AI can't do:** the AI cannot play a video or hear sound. Instead it looks at several still pictures and measures loudness as numbers. See [How the AI checks results](docs/checking-video-and-audio.md).

## Documents

| Document | Read it when |
|---|---|
| [Example requests](examples/conversations.md) | you want ideas for what to ask |
| [How the AI checks results](docs/checking-video-and-audio.md) | you want the AI to review its own work |
| [Troubleshooting](docs/troubleshooting.md) | the connection fails or you see an error |
| [Connecting other AI apps](examples/other-clients.md) | you use something other than Claude Desktop |
| [Tool reference](docs/tools.md), [Rules for the AI](docs/guide-for-ai.md) | you are a developer or want the details |
| [Changelog](CHANGELOG.md) | you want to know what changed |

## Questions and bug reports

Please post in [GitHub Issues](https://github.com/NexCreDesign/ai-effect-video-mcp/issues). It helps to include:

- The editor version: **Help › About AI Effect Video…** in the editor
- What you asked the AI and the error it showed
- The log file: save it with **Help › Save Log…** in the editor

## License

The documents and examples in this repository are under the [MIT License](LICENSE). The AI Effect Video editor (free edition) is free to use, but it is not under the MIT License and its source code is not public. It is governed by the [editor's license terms](https://github.com/NexCreDesign/AIEffectVideo_release/blob/main/LICENSE.md).