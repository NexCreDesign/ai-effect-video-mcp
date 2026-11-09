# Connecting other AI apps

[한국어](other-clients.ko.md)

Claude Desktop is covered in the [main guide](../README.md#step-2-connect-your-ai-app-claude-desktop). This page covers the others.

Every app needs one thing: **where `effectvideo-mcp.exe` is, inside the editor folder**. The examples assume the editor is in `C:\Tools\EffectVideo-1.0.0`, which is what you get by unzipping to `C:\Tools`. Use your own folder.

## ChatGPT desktop app (Work mode) and Codex

**Work** mode in the ChatGPT desktop app and Codex (the command-line program) use the same settings file.

1. Type `%USERPROFILE%\.codex` in the address bar of File Explorer to open the folder.
2. Open `config.toml` in Notepad, paste the following at the end, and save.

   ```toml
   [mcp_servers.ai-effect-video]
   command = 'C:\Tools\EffectVideo-1.0.0\effectvideo-mcp.exe'
   startup_timeout_sec = 30
   tool_timeout_sec = 120
   ```

   - In this file the path goes in single quotes `' '`, with each `\` written once.
   - The last two lines (30, 120) make it wait longer. Making a video file from a stretch of the edit takes time, and without them it may time out.
3. Quit the ChatGPT app (or Codex) completely and start it again.
4. In the ChatGPT app, choose **Work** at the top, then say "Show me the AI Effect Video status."

Good to know:

- It does not work in **Chat** mode of the ChatGPT app or on the ChatGPT website.
- Codex may ask "Allow?" before requests that change the edit or save files. Requests that only read, such as checking the status or looking at the picture, run without asking.

## Claude Code

Run one line in Command Prompt or a terminal:

```
claude mcp add ai-effect-video -- "C:\Tools\EffectVideo-1.0.0\effectvideo-mcp.exe"
```

## VS Code

In the folder you work in, create a `.vscode` folder and in it a file `mcp.json` with:

```json
{
  "servers": {
    "ai-effect-video": {
      "type": "stdio",
      "command": "C:\\Tools\\EffectVideo-1.0.0\\effectvideo-mcp.exe"
    }
  }
}
```

In a JSON file each `\` is written twice, as `\\`.

## Other apps

Most apps with a menu like "Add MCP server" will work. Enter:

| Field | Value |
|---|---|
| Command | The full path of `effectvideo-mcp.exe` |
| Arguments | Leave empty |
| Transport | stdio (local) |

## Advanced: environment variables

You normally need none. Use them only when something conflicts.

| Variable | Use |
|---|---|
| `EFFECTVIDEO_MCP_PORT` | The port number used to talk to the editor (default 9877). Change it if another program already uses 9877. Give the same value to the editor and to the AI app's settings. |
| `EFFECTVIDEO_APP` | Where `effect_video.exe` is, to start the editor when it is not running. By default, the one next to `effectvideo-mcp.exe`. |
