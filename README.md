# Video Subtitles & Summary

A portable assistant skill that extracts subtitles from video URLs with `yt-dlp` and summarizes them. It works with Claude Code, Codex, and Antigravity because the skill uses a standard `SKILL.md` file with YAML frontmatter and plain Markdown instructions.

## Installation

Copy the `video-subtitles-summary` folder, keeping the filename `SKILL.md`, into the skills directory for your assistant:

| Assistant | Project-level | User-level |
|---|---|---|
| Claude Code | `.claude/skills/video-subtitles-summary/` | `~/.claude/skills/video-subtitles-summary/` |
| Codex | `.agents/skills/video-subtitles-summary/` | `~/.agents/skills/video-subtitles-summary/` |
| Antigravity | `.agent/skills/video-subtitles-summary/` | `~/.agent/skills/video-subtitles-summary/` |

Reload the assistant if it does not discover the skill immediately.

## Requirements

- Install `yt-dlp` separately and make it available on `PATH`.
- `ffmpeg` is needed only for subtitle conversion to formats such as SRT.
- The skill does not download video content.

## Usage

Provide a video URL and ask for the subtitles, a transcript, or a summary. The skill checks available tracks, prefers authored captions, and uses auto-captions only when needed.
