---
name: trend-scout
description: Scout what is rising in a niche right now across TikTok, YouTube, Instagram Reels, Reddit and Threads - search phrases people type, trending videos, hashtags, fresh high-performing uploads and hot discussions - and turn it into a trend brief with content angles. Use when the user asks what is trending, what to post this week, rising topics or keywords in a niche, hashtag ideas, or early signals for a product category.
license: MIT
compatibility: Needs the Captapi MCP server or CAPTAPI_API_KEY (see the captapi skill).
metadata:
  author: Captapi
  version: "1.0.0"
  homepage: https://captapi.com
---

# Trend Scout

A trend is the same idea showing up in several places at once, recently, with more engagement than usual. This skill collects those signals for one niche and separates real trends from noise.

## Data access

Use Captapi through MCP tools (for example `tiktok_search_suggestions`) or REST (`https://api.captapi.com/v1/...` with `Authorization: Bearer $CAPTAPI_API_KEY`). Setup, errors and credit rules are in the `captapi` skill.

## Signals

| Signal | Endpoint | Credits | Useful parameters |
|---|---|---|---|
| What people type into TikTok search | `/v1/tiktok/search-suggestions` | 2 | `q`, `country`, `language` |
| TikTok keyword results | `/v1/tiktok/top-search` | 2 | `q` |
| TikTok trending videos | `/v1/tiktok/trending-feed` | 2 | `country`, `orderBy`, `period` |
| TikTok hashtags | `/v1/tiktok/popular-hashtags` | 2 | `country`, `period`, `query` |
| Fresh YouTube uploads | `/v1/youtube/search` | 2 | `q`, `uploadDate=this_week`, `sortBy=views` (only relevance or views), `type=videos` |
| Instagram Reels | `/v1/instagram/reels-search` | 2 | `q`, `datePosted` |
| Reddit discussions | `/v1/reddit/subreddit-search` | 2 | `url` of a niche subreddit, `q`, `sort=top`, `timeframe=week` |
| Threads conversation | `/v1/threads/search` | 2 | `q` |

A standard scan (suggestions + TikTok search + YouTube search + Reddit + Reels) is 5 calls, about 10 credits.

## Workflow

1. **Seed.** One niche or product category plus 1–3 seed keywords. Market (country) defaults to US.
2. **Estimate** and tell the user.
3. **Expand the keyword.** `tiktok/search-suggestions` gives the real phrases people search. Keep the 5–10 most specific ones ("protein snacks on the go" beats "protein").
4. **Collect evidence** for the best 2–3 phrases from at least three sources.
5. **Score each candidate trend** on:
   - **Recency:** most evidence from the last 7–14 days.
   - **Spread:** shows up on 2+ platforms.
   - **Engagement:** posts about it beat their creators' typical numbers, or Reddit threads have unusually many comments.
   - **Momentum words:** "new", "just dropped", "everyone is", "finally".
6. **Classify.** Rising (recent, multi-platform), steady (always popular), fading (old evidence only), single-source (one platform; watch it).
7. **Angles.** For each rising trend, 2–3 content or product angles the user could act on this week.

## Output

```markdown
# Trend brief: {niche} ({country}, {date})

## Search phrases people use
{phrase 1}, {phrase 2}, ...

## Trends
| Trend | Status | Evidence | Platforms |
|---|---|---|---|
| {trend} | Rising | [TikTok](url) {views} views in {n} days; [Reddit](url) {n} comments this week | TikTok, Reddit |

## Angles to use this week
1. ...

## Watch list (single-source)
- ...

Credits used: {n}
```

## Pitfalls

- One viral post is not a trend. Require two or more independent pieces of evidence.
- Evergreen topics look like trends. Check dates; if the evidence is months old, it is steady, not rising.
- Search and trending feeds are samples, not complete rankings. Do not claim "the #1 trend".
- Market matters. Suggestions and trending feeds differ by country; state the one you used.
- Site-wide Reddit search sorted by `top` matches loosely and returns viral off-topic posts. Search inside niche subreddits, or use `sort=relevance` site-wide, and drop off-topic results before counting.
