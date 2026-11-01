---
name: comment-mining
description: Mine comments on YouTube videos, TikToks, Reddit threads, Facebook posts or Instagram posts for repeated questions, objections, pain points, feature requests, buying intent and the exact words the audience uses. Use when the user wants voice-of-customer research, content ideas from what viewers ask, FAQ material, ad copy language, or to understand how people reacted to a video, post or launch.
license: MIT
compatibility: Needs the Captapi MCP server or CAPTAPI_API_KEY (see the captapi skill).
metadata:
  author: Captapi
  version: "1.0.0"
  homepage: https://captapi.com
---

# Comment Mining

Comments are the cheapest customer interviews on the internet. This skill pulls them, groups them into themes and keeps the audience's exact words.

## Data access

Use Captapi through MCP tools (for example `youtube_comments`) or REST (`https://api.captapi.com/v1/...` with `Authorization: Bearer $CAPTAPI_API_KEY`). Setup, errors and credit rules are in the `captapi` skill.

## Sources

| Platform | Endpoint | Credits | Notes |
|---|---|---|---|
| YouTube | `/v1/youtube/comments` | 2 per call | `limit` up to 500; `commentsDisabled: true` costs 0 |
| YouTube replies | `/v1/youtube/comment-replies` | 2 | Needs `commentId`; use for threads with many replies |
| TikTok | `/v1/tiktok/comments` | 2 per call | Includes `commentLanguage` |
| Reddit | `/v1/reddit/post-comments` | 2 per call | Returns the post body too, with `depth` and `parentId` |
| Facebook | `/v1/facebook/comments` | 2 | |
| Instagram | `/v1/instagram/comments` | ~0.9 per comment | 50 comments ≈ 45 credits. Ask before using |

To find which videos to mine, use the `outlier-posts` skill or `/v1/youtube/channel-videos` (2) and pick the most-viewed or most-commented items.

## Workflow

1. **Pick the posts.** 1–5 URLs. If the user gives a channel, choose its top 3 posts by views or comments and say why.
2. **Estimate.** Example: "3 YouTube videos × 1 call = 6 credits."
3. **Fetch** with `limit=100` per post (one call). Page only if the thread is large and the first page is not enough.
4. **Clean.**
   - De-duplicate by comment `id`; the same comment can come back twice in one page.
   - Drop comments where `text` is null, empty or only "[Photo]" (deleted or image-only).
   - Separate the creator's own comments (`authorIsChannelOwner` on YouTube, `isSubmitter` on Reddit). Pinned promo comments are not audience signal.
   - Remove spam, bots and one-word reactions from theme counts, but keep them in the total.
5. **Code each comment** into one or more buckets: question, pain point, objection, request (content or feature), praise, buying intent, comparison with an alternative, confusion.
6. **Cluster** into themes. Count how many comments each theme has and weight by likes or upvotes.
7. **Keep exact language.** For each theme, keep 2–3 verbatim quotes with a link.
8. **Turn it into action**: content ideas, FAQ entries, objections to answer on a landing page, words to reuse in ads.

## Output

```markdown
# Comment report: {video or post title}

Sources: {n} posts, {n} comments read ({n} after cleaning). Platforms: {list}.

## Top themes
| Theme | Type | Comments | Likes | Example |
|---|---|---:|---:|---|
| {theme} | question | 14 | 92 | "{verbatim quote}" ([link](url)) |

## What they keep asking
1. ...

## Objections and doubts
- ...

## Exact language to reuse
- "..."

## Ideas
- Video / post: ...
- FAQ or landing page: ...

Credits used: {n}
```

## Pitfalls

- Small samples. Under 30 usable comments, say the themes are directional.
- Over-reading likes on old comments; early comments collect more likes.
- Paraphrasing and calling it a quote. Quotes must be verbatim.
- Exposing commenters. Quote text with a link; do not compile personal details about individual commenters.
- Pulling Instagram comments at scale without asking; it is priced per comment.
- Trusting `commentLanguage` on very short comments. It mislabels some; check the text before splitting by language.
- TikTok photo carousels: if a `/photo/<id>` URL is rejected, retry with the same ID as `https://www.tiktok.com/@<handle>/video/<id>`.
