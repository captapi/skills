---
name: captapi
description: Data layer for public social media data through Captapi (YouTube, TikTok, Instagram, Reddit, X/Twitter, LinkedIn, Facebook, Threads, Bluesky, Pinterest, ad libraries and more). Use when a task needs live posts, profiles, comments, transcripts, search results or ads from a public URL or keyword, when the user mentions Captapi, or when another research skill needs to know which endpoint to call, what it costs and how to page through results.
license: MIT
compatibility: Needs network access plus either the Captapi MCP server (https://api.captapi.com/mcp) or a CAPTAPI_API_KEY environment variable for REST calls.
metadata:
  author: Captapi
  version: "1.0.0"
  homepage: https://captapi.com
---

# Captapi

Captapi returns structured JSON for public social media data. You send a public URL, @handle or search query; you get posts, profiles, comments, transcripts or ads back. One key works across every platform.

Use this skill to pick the right endpoint, call it correctly and keep the user's credit spend predictable. The workflow skills in this pack (`outlier-posts`, `comment-mining`, `competitor-teardown`, `ad-library-teardown`, `trend-scout`, `reddit-demand`, `video-to-content`, `creator-vetting`) rely on these rules.

## Setup

Pick one of these. Check which one is available before asking the user anything.

1. **MCP tools already connected.** If tools named like `youtube_channel_videos`, `tiktok_comments` or `reddit_search` exist, use them. Nothing else to set up.
2. **Hosted MCP server.** URL `https://api.captapi.com/mcp`. Clients that send headers (Claude Code, Cursor, VS Code) pass `Authorization: Bearer capt_live_...`. claude.ai and Claude Desktop add it as a custom connector and sign in with a Captapi account instead of a key.
3. **REST with a key.** If `CAPTAPI_API_KEY` is set, call the API directly:

```bash
curl -s "https://api.captapi.com/v1/youtube/channel-videos?url=@mkbhd&limit=30" \
  -H "Authorization: Bearer $CAPTAPI_API_KEY"
```

If none of these work, tell the user to create a free key at https://captapi.com/dashboard/api-keys (new accounts get 100 free credits) and stop. Never ask the user to paste a key into the chat; ask them to set it as an environment variable or connect the MCP server.

MCP tool names map to REST paths: `/v1/youtube/channel-videos` is `youtube_channel_videos`, `/v1/ad-library/facebook/company-ads` is `facebook_ad_library_company_ads`. Parameters are the same.

## Calling rules

- **Inputs.** Most endpoints take `url` (a public URL, @handle or ID for that platform) or `q` (a search query). Never pass a URL from one platform to another platform's endpoint.
- **Pagination.** List endpoints return `nextCursor` and `hasMore`. Pass `cursor=<nextCursor>` for the next page. Stop when `hasMore` is false or you have enough data.
- **Limits.** Start with the smallest `limit` that answers the question. `totalReturned` can be lower than `limit`; that is normal.
- **Errors.** Failures return `{"success": false, "error": {"code", "message", "retryable"}}`. Retry once only when `retryable` is true. Report the message to the user otherwise.
- **Slow calls.** Some calls (ad libraries, demographics, large comment pages) take 10–60 seconds. Wait; do not fire duplicates.
- **Pinned posts.** Profile feeds can include pinned posts that are months or years older than the rest. Check dates before computing baselines.
- **Large responses.** Transcripts of long videos and ad-library results with media can be very large. Request what you need, then work from the fields that matter.

## Credits

- Every endpoint has a fixed or per-result price. See `references/endpoints.md`.
- Failed calls (4xx/5xx) and empty results cost 0 credits.
- `cache=true` serves a 24-hour shared cache hit for 0 credits. Use it when rerunning the same research or when fresh-to-the-minute data does not matter.
- The `X-Captapi-Credits` response header shows the real charge. `account_balance` and `account_request_history` (0 credits) show balance and recent calls.
- `POST /v1/batch` runs up to 20 GET calls in one request; each item is billed like a direct call.

**Budget rule for every workflow:** before running more than 3 calls, tell the user the plan in one line with an estimated credit total (for example "5 calls, about 10 credits"). If a single step would cost more than 20 credits (per-result endpoints such as Instagram comments), ask first.

## What the data does not contain

- Public data only. No private accounts, no analytics for accounts the user does not own, no logged-in views.
- Ad libraries do not include spend, CTR or conversions for commercial ads. Never estimate them.
- Some counts are approximate; fields like `followersIsApproximate` or `publishedTimeIsApproximate` say so. Carry that caveat into the output.

## Output habits

- Link every post, video, comment or ad you cite.
- Quote text verbatim; never invent quotes.
- End with what the user can do next, plus the total credits used.

## Endpoint reference

`references/endpoints.md` lists the endpoints these skills use, with costs. The full list (190 endpoints, one line each) is at https://captapi.com/llms-endpoints.txt and every parameter is in https://captapi.com/openapi.json.
