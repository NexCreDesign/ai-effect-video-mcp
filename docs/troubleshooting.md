# Troubleshooting

[한국어](troubleshooting.ko.md)

## The editor doesn't start

- **A blue "Windows protected your PC" window appears.** The program is not code-signed yet. Click **More info**, then **Run anyway**.
- **It closes at once or shows a DLL error.** Make sure all files from the zip are in the same folder. Moving `effect_video.exe` alone does not work, and neither does running it from inside the zip without unzipping.

## The AI doesn't know about the editor

If the AI says "there is no such tool" or "I can't access AI Effect Video", the connection settings were not picked up.

1. After changing the settings, did you **quit the AI app completely** and start it again? If it stays in the system tray, it does not reread its settings.
2. Does the path in the settings match where `effectvideo-mcp.exe` really is? If you moved the folder, change the path too. If you unzipped into a folder already named `EffectVideo-1.0.0`, the program may be one level deeper: `...\EffectVideo-1.0.0\EffectVideo-1.0.0\`.
3. In Claude Desktop and VS Code settings (JSON), is every `\` written twice, as `\\`?
4. With ChatGPT, are you in **Work** mode of the desktop app? **Chat** mode and the website can't use it.

## The AI reports an error

If the AI mentions one of these words, do the following.

| Error | Meaning | What to do |
|---|---|---|
| `not_connected` | Could not reach the editor | Start the editor yourself and check that **Settings › Allow AI connection (MCP)** is on |
| `connection_lost` | The editor closed while working | Start the editor again and repeat your request |
| `timeout` | The editor didn't answer within a minute | If a window (such as a save prompt) is open in the editor, close it and ask again. The request was not carried out |
| `busy` | An export is running | Wait for it to finish or say "cancel the export" |
| `unsaved_changes` | There is unsaved work | Tell the AI whether to save it or throw it away |
| `file_exists` | A file with that name already exists | Tell the AI whether to replace it or use another name |
| `missing_media` | A video used by the project has been moved | Tell the AI where the file is now |
| `no_source` | No video is open yet | Tell the AI which video to open first |
| `clip_too_long` | Making the check file took too long | Ask for shorter stretches, or ask for an export instead |

Other errors are listed in the [tool reference](tools.md#error-codes).

## It conflicts with another program (advanced)

The editor talks to the AI on port 9877, inside this PC. If another program already uses that number, the connection fails. Set another number with the environment variable `EFFECTVIDEO_MCP_PORT`, the same for the editor and the AI app's settings. See [Connecting other AI apps](../examples/other-clients.md#advanced-environment-variables).

## Log file

**Help › Save Log…** in the editor saves a file that records what went wrong. Attach it when you ask for help. Logs are also kept in `%LOCALAPPDATA%\EffectVideo\logs`.

## Still stuck?

Post in [GitHub Issues](https://github.com/NexCreDesign/ai-effect-video-mcp/issues) with:

- The editor version (**Help › About AI Effect Video…**)
- Which AI app you use (for example Claude Desktop, ChatGPT desktop Work)
- What you asked the AI and the error it showed
- The log file
