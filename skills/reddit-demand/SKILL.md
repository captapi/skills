---
name: reddit-demand
description: Validate demand for a product, feature or content idea from Reddit - find threads where people describe the problem, ask for recommendations, complain about current tools or say what they would pay for - and summarize pains, workarounds, alternatives mentioned and exact phrasing, with links. Use when the user wants to validate an idea, find pain points, research a market, discover where their customers hang out, or find threads where their product would genuinely help.
license: MIT
compatibility: Needs the Captapi MCP server or CAPTAPI_API_KEY (see the captapi skill).
metadata:
  author: Captapi
  version: "1.0.0"
  homepage: https://captapi.com
---

# Reddit Demand

People on Reddit describe their problems in detail and name the tools that failed them. This skill turns those threads into a demand report.

## Data access

Use Captapi through MCP tools (for example `reddit_search`) or REST (`https://api.captapi.com/v1/...` with `Authorization: Bearer $CAPTAPI_API_KEY`). Setup, errors and credit rules are in the `captapi` skill.

## Endpoints

| Step | Endpoint | Credits | Notes |
|---|---|---|---|
| Site-wide search | `/v1/reddit/search` | 2 | `q`, `sort` (relevance, new, top, hot, comments), `timeframe` (hour … all, with top/comments) |
| Inside one subreddit | `/v1/reddit/subreddit-search` | 2 | `url` of the subreddit + `q` |
| Browse a subreddit | `/v1/reddit/subreddit-posts` | 2 | `sort`, `timeframe` |
| Read a thread | `/v1/reddit/post-comments` | 2 | Returns the post body and a flat comment list with `depth`, `parentId`, `isSubmitter` |
| Subreddit size and rules | `/v1/reddit/subreddit-details` | 1 | Check rules before suggesting anyone post there |

Search results include title, subreddit, score and comment count, but often not the post body. Read the body with `post-comments`.

## Workflow

1. **Frame the problem, not the product.** Turn "AI note-taking app" into the phrases a person with the problem would write: "alternative to {tool}", "how do you manage {task}", "{tool} too expensive", "is there a tool that".
2. **Estimate.** Example: "4 searches + 5 threads = 18 credits."
3. **Search** 3–5 phrasings with `sort=relevance` site-wide. For the strongest threads, use `sort=top&timeframe=year` only inside a relevant subreddit (`subreddit-search`); site-wide `top` returns viral posts that merely share a word with the query. Drop off-topic results before shortlisting.
4. **Shortlist** 5–10 threads: real problem described, recent enough to matter, decent comment count. Skip promotional posts.
5. **Read** each shortlisted thread with `post-comments` (`limit=50`).
6. **Extract** per thread:
   - the problem in the poster's words,
   - current workaround or tool and why it falls short,
   - must-have requirements,
   - alternatives others recommend (and the pushback they get),
   - price or willingness-to-pay signals,
   - urgency (deadline, cost of the problem).
7. **Synthesize** across threads: the top pains by frequency, the gaps current tools leave and the exact phrases to reuse in positioning.
8. **Where to show up.** Subreddits that keep surfacing, with their rules summarized.

## Output

```markdown
# Demand report: {problem space}

Searched: {phrases}. Threads read: {n} ({n} comments). Window: {dates}.

## Top pains
| Pain | Threads | Example (verbatim) |
|---|---:|---|
| {pain} | 6 | "{quote}" ([thread](url)) |

## Current tools and why they fall short
- **{tool}**: ...

## Must-haves people list
- ...

## Willingness to pay
- ...

## Where these people are
| Subreddit | Why | Self-promo rules |
|---|---|---|

## Positioning language
- "..."

Credits used: {n}
```

## Pitfalls

- Reading only titles. The detail is in the post body and the replies.
- Counting the original poster's own replies as separate demand. Use `isSubmitter`.
- Deleted comments come back with `text: null`. Skip them.
- Presenting a handful of threads as market size. This is qualitative evidence; say so.
- Spam. If the user wants to reply in threads, suggest genuinely helpful answers that follow each subreddit's rules, with disclosure when they mention their own product. Do not draft mass or copy-paste replies.
