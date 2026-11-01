---
name: outlier-posts
description: Find the videos, Reels, Shorts, TikToks or posts that beat a creator's or brand's own normal performance, explain why they worked and turn them into a swipe file of hooks, formats and title patterns. Use when the user asks for outliers, viral posts, top-performing videos, what is working for a channel, or title and hook ideas based on a competitor's best content on YouTube, TikTok, Instagram, X/Twitter, LinkedIn or Threads.
license: MIT
compatibility: Needs the Captapi MCP server or CAPTAPI_API_KEY (see the captapi skill).
metadata:
  author: Captapi
  version: "1.0.0"
  homepage: https://captapi.com
---

# Outlier Posts

Raw view counts say who is big. Outliers say what worked. This skill scores every recent post against the account's own median and explains the posts that broke out.

## Data access

Use Captapi through MCP tools (for example `youtube_channel_videos`) or REST (`https://api.captapi.com/v1/...` with `Authorization: Bearer $CAPTAPI_API_KEY`). Setup, errors and credit rules are in the `captapi` skill.

## Feeds by platform

| Platform | Endpoint | Credits | Notes |
|---|---|---|---|
| YouTube long-form | `/v1/youtube/channel-videos` | 2 per call | `limit` up to 200 in one call |
| YouTube Shorts | `/v1/youtube/channel-shorts` | 2 per call | Score Shorts separately from long-form |
| TikTok | `/v1/tiktok/channel-posts` | 2 per call | Metrics are in `engagement` (views, likes, comments, shares, saves) |
| Instagram Reels | `/v1/instagram/channel-reels` | ~0.3 per Reel | 30 Reels ≈ 9 credits |
| X/Twitter | `/v1/twitter/user-tweets` | 2 | Returns the most popular tweets, not a chronological feed. Say so; the baseline is biased upward |
| LinkedIn company | `/v1/linkedin/company-posts` | 1 | Only ~10 posts; confidence is low |
| Threads | `/v1/threads/user-posts` | 2 | Engagement includes views when exposed |

Enrichment for the top outliers only: `/v1/youtube/video-details` (1), `/v1/youtube/transcript` (1), `/v1/tiktok/transcript` (2), `/v1/youtube/comments` or `/v1/tiktok/comments` (2).

## Workflow

1. **Scope.** Accounts (URL or @handle), platform, window. Default: last 30 posts per account. Ask only if the account is missing.
2. **Estimate and tell the user.** Example: "2 channels × 1 call = 4 credits, plus up to 3 transcripts = 3 credits."
3. **Fetch.** One call with `limit=30` is usually enough. Page with `cursor` only if the user wants a longer window.
4. **Clean the sample.**
   - Drop posts younger than 72 hours; they have not finished collecting views. List them separately as "too new to score".
   - Drop posts far outside the window. Feeds often include pinned posts (old viral posts at the start or end of the list, months or years older than the rest). They wreck the baseline.
   - Split formats that behave differently (Shorts vs long-form, Reels vs carousels, live streams). On TikTok, photo carousels have `/photo/` URLs and `mediaType: "photo"`; score them separately from videos and say which format the outliers are.
5. **Baseline.** Median of the primary metric per account and per format. Views for video platforms; likes + comments + reposts for text platforms. Never use the mean.
6. **Score.** `lift = metric / median`.
   - 5x or more: huge outlier
   - 2x to 5x: strong outlier
   - 1.5x to 2x: mild outlier
   - Fewer than 10 scored posts: mark confidence low.
7. **Explain the winners.** For the top 3–5, look at title or caption, hook (first line or first 10 seconds of the transcript), topic, format, length, timing, and what commenters reacted to. Fetch a transcript or comments only when the title alone does not explain it.
8. **Find the pattern.** What do the outliers share that the median posts do not? Name the pattern, show the evidence, and note counter-examples.

## Output

```markdown
# Outliers: {account} ({platform})

Sample: {n} posts, {date range}. Median {metric}: {value}. Bar for outlier: 2x = {value}. Confidence: {high/medium/low}.

| # | Post | Published | {metric} | Lift | Why it likely worked |
|---:|---|---|---:|---:|---|
| 1 | [title](url) | {date} | 1,216,736 | 20.6x | Promises the hidden mechanism + broad topic |

## What the outliers have in common
1. **{pattern}**: evidence from 3+ posts.

## Reusable formulas
- "{formula}" → example for the user's niche

## Too new to score
- [title](url), published {date}

Credits used: {n}
```

## Pitfalls

- Comparing a 5M-subscriber channel with a 20k one on raw views. Always compare each account with itself.
- Mixing platforms or formats into one baseline.
- Treating X/Twitter `user-tweets` as a random sample. It is a "most popular" list.
- Quoting a hook you did not read. Fetch the transcript or quote only the title or caption.
- Calling a pattern from one post. Two or more outliers, or say it is a hypothesis.
