# Changelog

What changed in each version of AI Effect Video, as far as AI connection is concerned. Details of every tool are in [docs/tools.md](docs/tools.md).

## 1.0.0

First public release: a free edition that runs from a zip, without installing.

**For users**

- Connect Claude Desktop, Claude Code, Codex (including Work mode in the ChatGPT desktop app), VS Code and other MCP apps.
- Ask the AI to open videos, join several videos, cut their ends, reorder them and add fades; sync effect videos; add text, mosaics and pictures; open subtitles; check the result with pictures and sound levels; save and export.
- The AI asks before it throws away unsaved work or replaces a file. Each request is one Ctrl+Z.
- Exports from the free edition carry a small watermark.

**For developers**

- 23 tools: status and project (`get_status`, `get_project`, `get_objects_at`, `get_subtitles`, `list_recent_projects`, `list_fonts`, `measure_text`), viewing (`look`, `look_range`), clips (`get_clip`, `get_audio`), frames (`frame_at_time`), editing (`edit_project`, `history`), files (`open_media`, `close_effect`, `save_project`, `probe_media`), playback and view (`playback`, `set_view`), export (`export`, `export_status`, `cancel_export`).
- MCP tool annotations: `readOnlyHint` on the 12 tools that only read, `destructiveHint` on tools that can replace files or drop work.
- `get_clip` returns `duration_seconds` (length of the clip) and `elapsed_seconds` (time it took); `get_audio` returns `duration_seconds`; `export_status` returns `elapsed_seconds` when finished.
- `get_status` reports `edition` as `free`, `trial` or `full`.
