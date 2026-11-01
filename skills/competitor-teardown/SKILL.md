---
name: competitor-teardown
description: Compare 2 to 5 competitor channels, creators or brands on YouTube, TikTok, Instagram, X/Twitter, LinkedIn or Threads - audience size, posting cadence, formats, topics, each account's breakout posts and what their audience asks for - and find the gaps nobody covers. Use when the user wants a competitor analysis, a content strategy benchmark, a "who is winning in my niche" brief, or video and post ideas based on what competitors do and miss.
license: MIT
compatibility: Needs the Captapi MCP server or CAPTAPI_API_KEY (see the captapi skill).
metadata:
  author: Captapi
  version: "1.0.0"
  homepage: https://captapi.com
---

# Competitor Teardown

A side-by-side brief on how a set of competitors publish, what breaks out for each of them and what their audiences still want.

## Data access

Use Captapi through MCP tools (for example `youtube_channel_details`) or REST (`https://api.captapi.com/v1/...` with `Authorization: Bearer $CAPTAPI_API_KEY`). Setup, errors and credit rules are in the `captapi` skill.

## Calls per competitor

| Step | YouTube | TikTok | Instagram |
|---|---|---|---|
| Profile | `/v1/youtube/channel-details` (1) | `/v1/tiktok/channel-details` (1) | `/v1/instagram/channel-details` (1) |
| Recent posts | `/v1/youtube/channel-videos` (2) | `/v1/tiktok/channel-posts` (2) | `/v1/instagram/channel-reels` (~0.3 each) |
| Audience voice (best post only) | `/v1/youtube/comments` (2) | `/v1/tiktok/comments` (2) | `/v1/instagram/comments` (~0.9 each, ask first) |

Other platforms: `/v1/twitter/profile` + `/v1/twitter/user-tweets`, `/v1/linkedin/company` + `/v1/linkedin/company-posts`, `/v1/threads/profile` + `/v1/threads/user-posts`.

Typical cost: 3 YouTube channels with comments = 3 × (1 + 2 + 2) = 15 credits.

## Workflow

1. **Scope.** The competitor list (URLs or @handles), platform and, if given, the user's own account to compare against.
2. **Estimate and tell the user** the call count and credits.
3. **Profiles.** Followers or subscribers, total posts, bio, links, verification.
4. **Recent posts.** Last 10–30 per account. From these compute:
   - cadence (posts per week over the window),
   - median views and median engagement,
   - format mix (Shorts vs long-form, video vs carousel, length buckets),
   - topic clusters from titles and captions.
5. **Breakouts.** Each account's best post against its own median (same method as the `outlier-posts` skill). This shows who is breaking out, which raw size hides.
6. **Audience voice.** Comments on each account's best post: top 2–3 questions or requests (method in `comment-mining`). Skip this call for accounts whose median post has fewer than 5 comments; there is nothing to read and it saves credits.
7. **Gaps.** Topics the audiences ask for that none of the accounts has covered in the window. Check titles before claiming a gap.
8. **Recommendations.** 3–5 concrete ideas, each tied to evidence (a breakout, a repeated question, a gap).

## Output

```markdown
# Competitor teardown: {niche}

| | {A} | {B} | {C} |
|---|---:|---:|---:|
| Followers | | | |
| Posts / week (last {n}) | | | |
| Median views | | | |
| Best post | [title](url) | | |
| Best post lift | 1.6x | 3.0x | 3.7x |
| Main formats | | | |

## What each audience asks for
- **{A}:** ... ([example](url))

## Patterns that work across the group
1. ...

## Gaps
1. {topic}: asked for in {n} comments, not covered in the last {n} uploads of any account.

## Ideas
1. ...

Credits used: {n}
```

## Pitfalls

- Declaring a winner by follower count. Lift against each account's own median is the fairer comparison.
- Brand-new posts dragging the median down, and pinned old posts pushing it up. Drop posts younger than 72 hours and posts far outside the window.
- Comparing windows of different lengths silently. A slow poster's 30 posts can span six months; report each account's window.
- A gap that is only a gap in the sampled window. Say "in the last {n} uploads".
- Comment samples under 30 are directional. Say so.
- No access to private analytics (watch time, CTR, revenue). Do not guess them.
