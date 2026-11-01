# Captapi Skills: social media research for AI agents

Research workflows your agent can run on live, public social media data: find a channel's outlier videos, mine comments for what people ask, tear down a competitor's ads, scout trends, validate demand on Reddit, repurpose videos and vet creators.

Each skill is a plain `SKILL.md` file that follows the [Agent Skills](https://agentskills.io) format, so it works in Claude Code, Cursor, Codex, Gemini CLI, VS Code and other agents that support skills. The data comes from [Captapi](https://captapi.com/?utm_source=github&utm_medium=readme&utm_campaign=skills): one API key for YouTube, TikTok, Instagram, Reddit, X/Twitter, LinkedIn, Facebook, Threads, ad libraries and more.

## Install

```bash
npx skills add CDCStream/captapi-skills
```

Claude Code plugin:

```text
/plugin marketplace add CDCStream/captapi-skills
/plugin install captapi-skills@captapi-skills
```

Or copy any folder from `skills/` into your agent's skills directory (for Claude Code: `~/.claude/skills/`).

## Connect the data

Pick one:

- **Hosted MCP server (recommended).** Add `https://api.captapi.com/mcp` to your agent. Claude Code, Cursor and VS Code send your key as `Authorization: Bearer capt_live_...`. In claude.ai and Claude Desktop, add it as a custom connector and sign in with your Captapi account; there is no key to paste.
- **REST.** Set `CAPTAPI_API_KEY` in your environment. The skills fall back to `curl` against `https://api.captapi.com`.

Get a key at [captapi.com](https://captapi.com/dashboard/api-keys?utm_source=github&utm_medium=readme&utm_campaign=skills). New accounts get 100 free credits. Setup snippets for each client are in the [integration docs](https://captapi.com/docs/integrations?utm_source=github&utm_medium=readme&utm_campaign=skills).

## Skills

| Skill | Ask your agent | You get | Typical credits |
|---|---|---|---:|
| [`outlier-posts`](skills/outlier-posts) | "Find the outlier videos on @channel and tell me what their titles have in common." | Outlier table scored against the account's own median, patterns, reusable formulas | 2–5 |
| [`comment-mining`](skills/comment-mining) | "Read the comments on these 3 videos. What do viewers keep asking for?" | Themes, objections, requests, verbatim quotes, content ideas | 2 per post |
| [`competitor-teardown`](skills/competitor-teardown) | "Compare these 3 YouTube channels and find a video none of them has made." | Side-by-side table, each account's breakout, audience asks, gaps | ~8–15 for 3 accounts |
| [`ad-library-teardown`](skills/ad-library-teardown) | "What Meta ads is Gymshark running right now, and which have run longest?" | Longest-running ad concepts, angles, hooks, offers, test ideas | 4 per brand |
| [`trend-scout`](skills/trend-scout) | "What's rising in protein snacks on TikTok, YouTube and Reddit this week?" | Search phrases, rising vs steady trends with evidence, angles | ~6–10 |
| [`reddit-demand`](skills/reddit-demand) | "Find Reddit threads where people want a Notion alternative for small teams." | Top pains, current tools and their gaps, willingness to pay, exact phrasing | ~8–20 |
| [`video-to-content`](skills/video-to-content) | "Turn this YouTube video into an X thread and a LinkedIn post." | Hook, structure with timestamps, quotes, drafts | 1–4 per video |
| [`creator-vetting`](skills/creator-vetting) | "Vet these 5 TikTok creators for a US skincare campaign." | Reach, engagement, consistency, audience country mix, fit scorecard | ~6 per creator |
| [`captapi`](skills/captapi) | Used by the others | Setup, calling rules, endpoint and cost reference | 0 |

## Real example outputs

Every skill was run on live data on 29 Oct 2026. The reports are in [`examples/`](examples), with the exact credits each run used:

| Example | Credits |
|---|---:|
| [Duolingo's outlier TikToks](examples/outlier-posts-duolingo-tiktok.md) | 2 |
| [What commenters say on Duolingo's 6M-view TikTok](examples/comment-mining-duolingo-outlier.md) | 2 |
| [Duolingo vs Babbel vs Busuu on TikTok](examples/competitor-teardown-language-apps.md) | 8 |
| [Gymshark vs Alphalete Meta ads](examples/ad-library-teardown-gymshark-alphalete.md) | 6 |
| [What's rising in "learn Spanish" this week](examples/trend-scout-learn-spanish.md) | 6 |
| [Why people leave Notion, from Reddit](examples/reddit-demand-notion-alternative.md) | 8 |
| [YouTube video to notes and a LinkedIn post](examples/video-to-content-ai-agency.md) | 1 |
| [Vetting two Spanish-teacher creators](examples/creator-vetting-spanish-teachers.md) | 13 |

## How the skills behave

- **They tell you the cost first.** Before running more than a few calls, the agent states the plan and an estimated credit total. Steps priced per result (like Instagram comments) need your OK.
- **They compare like with like.** Performance is judged against each account's own median, not raw follower counts.
- **They cite everything.** Every post, comment and ad in the output links to its source. Quotes are verbatim.
- **They stay honest about the data.** Public data only. No spend or CTR for commercial ads, and no private analytics. Small samples are labeled as such.

## Pricing in one paragraph

Every Captapi endpoint has a fixed or per-result price, listed in [`skills/captapi/references/endpoints.md`](skills/captapi/references/endpoints.md). Failed calls and empty results cost nothing, and `cache=true` reruns within 24 hours are free. See [pricing](https://captapi.com/pricing?utm_source=github&utm_medium=readme&utm_campaign=skills).

## Contributing

Issues and pull requests are welcome: new workflows, better output templates, fixes when an endpoint changes. Keep skills workflow-first (a business outcome, not an endpoint wrapper), cite sources, and never add claims the data cannot support. See [AGENTS.md](AGENTS.md).

## License

MIT. See [LICENSE](LICENSE).
