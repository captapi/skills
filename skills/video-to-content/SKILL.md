---
name: video-to-content
description: Turn YouTube videos, Shorts, TikToks or Instagram Reels into written content - pull the transcript or summary, extract the hook, key points, claims and quotable lines, then draft an X thread, LinkedIn post, newsletter section, blog outline or short-form scripts. Use when the user wants to repurpose a video, summarize what a video says, get quotes or timestamps from a video, or study how a video is structured.
license: MIT
compatibility: Needs the Captapi MCP server or CAPTAPI_API_KEY (see the captapi skill).
metadata:
  author: Captapi
  version: "1.0.0"
  homepage: https://captapi.com
---

# Video to Content

Get the words out of a video once, then reuse them in the formats the user needs.

## Data access

Use Captapi through MCP tools (for example `youtube_transcript`) or REST (`https://api.captapi.com/v1/...` with `Authorization: Bearer $CAPTAPI_API_KEY`). Setup, errors and credit rules are in the `captapi` skill.

## Endpoints

| Source | Transcript | Summary | Notes |
|---|---|---|---|
| YouTube | `/v1/youtube/transcript` (1) | `/v1/youtube/summarize` (3) | Uses published or auto captions. A caption miss returns 404 and costs 0 |
| YouTube, no captions | `/v1/youtube/audio-transcript` (2 per minute of audio) | | A 20-minute video is 40 credits. Ask first |
| YouTube Shorts | `/v1/youtube/shorts/transcript` (1) | `/v1/youtube/shorts/summarize` (3) | Shorts only (3 minutes or less) |
| TikTok | `/v1/tiktok/transcript` (2) | `/v1/tiktok/summarize` (4) | |
| Instagram Reels | `/v1/instagram/transcript` (2) | `/v1/instagram/summarize` (4) | |

Transcript responses contain the full text (`transcript`) plus `segments` with `startMs` and `endMs`. A long video's transcript is large; work from it in chunks.

**Which to call:** summary when the user only needs the gist of several videos; transcript when they need quotes, timestamps, structure or a rewrite.

## Workflow

1. **Inputs.** Video URL(s) and the target format(s). Ask whose video it is if that is unclear (see Rights below).
2. **Estimate** and tell the user.
3. **Fetch** the transcript (or the summary, per the rule above). Pass `language` if the user wants a specific caption track.
4. **Map the video.**
   - Hook: the first 15–30 seconds, verbatim.
   - Structure: sections with start timestamps.
   - Key points, claims (with who made them), numbers, examples.
   - 3–5 quotable lines, verbatim, with timestamps.
5. **Draft the formats.**
   - X thread: hook post + 5–8 posts, one idea each.
   - LinkedIn post: 1 insight, a short story or example, 1 takeaway; 150–250 words.
   - Newsletter section: headline, 3 key points, link to the video.
   - Blog outline: H2s from the video's sections, with timestamps as sources.
   - Short-form scripts: 2–3 scripts of 30–45 seconds, each built on one point.
6. **Fact hygiene.** Keep claims attributed to the speaker. Flag numbers you could not verify.

## Output

```markdown
# {video title}
Source: [link](url) · {duration} · transcript: {captions / audio}

## Hook
"{verbatim first lines}"

## Structure
- 00:00 {section}
- 02:15 {section}

## Quotable lines
- "{quote}" (03:42)

## {Format}
{draft}

Credits used: {n}
```

## Rights

- Repurposing the user's own videos, or videos they have permission to use: go ahead.
- Someone else's video: use it for research, summaries with a link, and short attributed quotes. Do not produce a near-verbatim copy for the user to publish as their own.

## Pitfalls

- Auto-captions mishear names and brands and insert tags like "[music]". Strip the tags from quotes and flag likely errors instead of repeating them.
- Captions are not speech-to-text. If `/v1/youtube/transcript` returns 404, the video has no captions; offer the audio transcript with its cost.
- Inventing timestamps. Use `segments`.
