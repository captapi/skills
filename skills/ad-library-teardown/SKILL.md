---
name: ad-library-teardown
description: Tear down a competitor's live ads from the Meta (Facebook and Instagram), Google, LinkedIn and TikTok ad libraries - which ads have run the longest, the angles, hooks, offers, CTAs, formats and landing pages - and turn them into test ideas. Use when the user asks what ads a brand is running, wants competitor ad research, creative inspiration, a swipe file of long-running ads, or to compare messaging across competitors.
license: MIT
compatibility: Needs the Captapi MCP server or CAPTAPI_API_KEY (see the captapi skill).
metadata:
  author: Captapi
  version: "1.0.0"
  homepage: https://captapi.com
---

# Ad Library Teardown

Ad libraries show what a brand is paying to run. They do not show spend or results, so the best public signal is longevity: ads that keep running for weeks are usually the ones that work. This skill finds them and breaks down the message.

## Data access

Use Captapi through MCP tools (for example `facebook_ad_library_company_ads`) or REST (`https://api.captapi.com/v1/...` with `Authorization: Bearer $CAPTAPI_API_KEY`). Setup, errors and credit rules are in the `captapi` skill.

## Sources

| Library | Find the advertiser | Get the ads | Notes |
|---|---|---|---|
| Meta | `/v1/ad-library/facebook/search-companies?q=<brand>` (2) | `/v1/ad-library/facebook/company-ads?url=<pageId>&status=ACTIVE&country=US` (2) | Pass the `pageId` from search-companies. Pick the right page: brands often have several (main, women's, regional) |
| Meta keyword | | `/v1/ad-library/facebook/search?q=<keyword>` (2) | For a category instead of one brand |
| Google | `/v1/ad-library/google/advertiser-search?q=<brand>` (1) | `/v1/ad-library/google/company-ads` (2) | Spend and impressions only exist for election ads |
| LinkedIn | | `/v1/ad-library/linkedin/search-ads` (2) | Run dates and impressions only on some creatives |
| TikTok | | `/v1/ad-library/tiktok/search?q=<keyword>` (2, 5 via fallback) | Covers EU/EEA, UK and Switzerland only. Say this whenever you use it |

Fields on Meta ads include `text`, `headline`, `cta`, `landingUrl`, `adFormat` (IMAGE, VIDEO, DCO, DPA and others), `firstShown`, `lastShown`, `publisherPlatforms`, `cards`, `videos`, `brandedContent` (creator partnership) and `variantCount`. `impressions` and `spend` are null for commercial ads.

## Workflow

1. **Scope.** Brands, library (default Meta), country (default US). Confirm the page when search-companies returns several.
2. **Estimate.** Example: "3 brands × (search 2 + ads 2) = 12 credits."
3. **Fetch active ads** with `limit=20`. Responses are large because of media URLs; keep the text fields and drop the rest from your working notes.
4. **Days running** = today − `firstShown` (dates are calendar days). Rank by it.
   - 30+ days: proven
   - 14–29 days: likely working
   - under 7 days: new test
   - If every active ad started within the last month, the brand rotates by campaign. Rank by relative age, say so, and optionally pull `status=INACTIVE` to see how long past concepts ran.
5. **Group duplicates.** Same text and headline with different images is one concept with variants. Count concepts, not rows.
6. **Break down each concept.**
   - Angle (pain, aspiration, social proof, price, novelty, authority)
   - Hook (first line of `text`, or the first seconds of a video)
   - Offer (discount, free shipping, bundle, trial)
   - CTA and landing page type (product page, collection, campaign page)
   - Format (static, video, carousel, catalog/DPA) and creator partnerships (`brandedContent`)
7. **Patterns** across the brand or across competitors.
8. **Test ideas.** 3–5 ads the user could test, each tied to a long-running competitor concept.

## Output

```markdown
# Ad teardown: {brand} (Meta, {country})

Active ads returned: {n} ({n} concepts). Library total shown by Meta: {searchResultsCount}.

## Longest-running concepts
| Concept | Days running | Format | Hook | Offer / CTA | Landing page | Ad |
|---|---:|---|---|---|---|---|
| Best sellers | 28 | DPA | "The pieces everyone's talking about." | Shop now | /collections/must-have | [link](url) |

## Angles they rely on
1. ...

## Creator and celebrity partnerships
- ...

## What to test
1. ...

Notes: public ad library data. No spend or performance data for commercial ads. Days running is a proxy, not proof of results.
Credits used: {n}
```

## Pitfalls

- Inventing spend, ROAS, CTR or "best performing" claims. The data has none of that for commercial ads.
- Counting variants as separate winners.
- Treating `retrievableCount` (what the API can page through) as the brand's total. `searchResultsCount` is Meta's own count. For big brands only about 30 active ads are reachable: Meta's own top ads for that page, in the order Meta ranks them (by impressions), not the newest ones. When `searchResultsCount` is much larger than `retrievableCount`, say that the analysis covers the brand's highest-reach active ads, not every ad it runs.
- Relying on media URLs later. They expire (`mediaUrlsExpireAt`); save the text, link to the ad page.
- Using the TikTok library for US brands. It only covers EU/EEA, UK and Switzerland.
- Calls can take 10–40 seconds. Wait instead of retrying.
