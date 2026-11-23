---
name: video-subtitles-summary
description: Extract subtitles or transcripts from a video URL with yt-dlp, then summarize them. Use when the user provides a video link and asks for subtitles, a transcript, notes, or a summary.
---

# Video subtitles and summary

Extract available subtitles from a video URL with `yt-dlp`, then produce a clear, faithful summary from the downloaded text. These plain Markdown instructions work in Claude Code, Codex, and Antigravity.

## Inputs and defaults

- Take the video URL from the user. If none was provided, ask for it.
- Write the summary in the user's language unless they request another.
- Prefer authored subtitles in the requested language. If unavailable, use automatic captions and say so.
- If no language was specified, inspect the available tracks and choose the video's original language when clear; otherwise prefer the user's language.
- Do not download the video unless explicitly asked. Do not bypass access controls, DRM, or login restrictions.

## Procedure

1. Check whether `yt-dlp` is installed with `yt-dlp --version`. If it is missing, explain how to install it; do not install software without authorization.
2. Inspect available subtitle tracks:

   ```bash
   yt-dlp --list-subs "VIDEO_URL"
   ```

3. Download only the chosen subtitle track, preferring authored captions:

   ```bash
   yt-dlp --skip-download --write-subs --sub-langs "LANG" --sub-format vtt --output "%(title).180B-%(id)s.%(ext)s" "VIDEO_URL"
   ```

   If authored captions are unavailable in that language, try automatic captions:

   ```bash
   yt-dlp --skip-download --write-auto-subs --sub-langs "LANG" --sub-format vtt --output "%(title).180B-%(id)s.%(ext)s" "VIDEO_URL"
   ```

   Use a language code such as `fr` or `en`, or one shown by `--list-subs`. Avoid downloading duplicate tracks. For a playlist URL, process only the requested video or clarify whether the user wants the whole playlist.
4. Read the resulting `.vtt` file. Ignore headers, cue numbers, timestamps, and formatting tags in the summary. Preserve names, numbers, dates, and qualifications. Remove only obvious repeated cue fragments; do not silently repair uncertain or garbled text.
5. Unless the user requests another format or length, provide:
   - a 2–4 sentence overview;
   - concise bullets covering the main ideas;
   - important conclusions or recommendations;
   - reliable timestamps for notable sections.
6. State whether captions were authored or automatically generated. Note significant gaps or errors, distinguish the speaker's claims from verified facts, and do not invent missing content.
7. Keep subtitle files separate. If the user asks for a cleaned transcript or saved summary, create a separate file and avoid overwriting existing files.
8. Report subtitle language, caption source, and output location. If extraction fails or captions are unavailable, explain why and offer to summarize a transcript the user provides.

## Examples

```bash
# Inspect available subtitle tracks
yt-dlp --list-subs "https://www.youtube.com/watch?v=VIDEO_ID"

# French authored subtitles, without downloading the video
yt-dlp --skip-download --write-subs --sub-langs "fr" --sub-format vtt "https://www.youtube.com/watch?v=VIDEO_ID"

# French automatic captions, if authored French subtitles are unavailable
yt-dlp --skip-download --write-auto-subs --sub-langs "fr" --sub-format vtt "https://www.youtube.com/watch?v=VIDEO_ID"

# Convert subtitles to SRT if requested (requires FFmpeg)
yt-dlp --skip-download --write-subs --sub-langs "fr" --convert-subs srt "https://www.youtube.com/watch?v=VIDEO_ID"
```

## Working style

- Be transparent when captions are machine-generated or incomplete.
- A summary is not a verbatim transcript. Use only short quotations when useful and requested.
- Respect copyright and the source service's terms; process only material the user is entitled to access.
