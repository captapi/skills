---
name: creator-vetting
description: Vet TikTok, Instagram or YouTube creators before a sponsorship, UGC deal or affiliate partnership - real engagement against follower count, consistency of views, likely country and language of the audience, content fit, brand safety signals and public contact links - and return a scorecard. Use when the user wants to check an influencer, compare creators for a campaign, build a shortlist, or spot fake or inflated engagement.
license: MIT
compatibility: Needs the Captapi MCP server or CAPTAPI_API_KEY (see the captapi skill).
metadata:
  author: Captapi
  version: "1.0.0"
  homepage: https://captapi.com
---

# Creator Vetting

Follower counts are easy to inflate and easy to misread. This skill checks what a creator's recent posts actually do and who seems to be watching.

## Data access

Use Captapi through MCP tools (for example `tiktok_channel_details`) or REST (`https://api.captapi.com/v1/...` with `Authorization: Bearer $CAPTAPI_API_KEY`). Setup, errors and credit rules are in the `captapi` skill.

## Calls per creator

| Check | TikTok | Instagram | YouTube |
|---|---|---|---|
| Profile | `/v1/tiktok/channel-details` (1) | `/v1/instagram/channel-details` (1) | `/v1/youtube/channel-details` (1) |
| Recent posts | `/v1/tiktok/channel-posts` (2) | `/v1/instagram/channel-reels` (~0.3 each) | `/v1/youtube/channel-videos` (2) |
| Audience location and language | `/v1/tiktok/audience-demographics` (3) | | |
| Creator's own region | `/v1/tiktok/profile-region` (2) | | |
| Comment quality (1–2 posts) | `/v1/tiktok/comments` (2) | `/v1/instagram/comments` (~0.9 each, ask first) | `/v1/youtube/comments` (2) |
| Contact and links | `/v1/linktree/page` (1) when the bio links to Linktree | | |

Discovery (finding creators rather than checking them): `/v1/tiktok/popular-creators` (2) or `/v1/tiktok/search/users` (1).

A full TikTok check is about 1 + 2 + 3 + 2 = 8 credits per creator.

## Workflow

1. **Inputs.** Creator handles, the brand or product, target market (country and language), campaign type.
2. **Estimate** and tell the user.
3. **Profile.** Followers, total posts, verified, private, bio link, category, and whether counts are approximate (`followersIsApproximate`).
4. **Recent performance** from the last 20–30 posts (drop posts younger than 72 hours and pinned old posts that sit far outside the window):
   - median views,
   - median views ÷ followers (reach rate),
   - median (likes + comments) ÷ median views (engagement rate),
   - consistency: share of posts within 0.5x–2x of the median. Spiky accounts depend on rare hits.
5. **Audience.** On TikTok, `audience-demographics` gives country mix (from repliers) and language mix (from commenters) with a `confidence` value. Compare with the target market. Report `sampleSize` and confidence; it is not a follower census. If the country sample is under about 50, call the country mix unusable and rely on the language mix (`languageSampleSize`) instead.
6. **Comment quality.** On 1–2 recent posts: real sentences and questions vs generic emoji, repeated phrases or off-topic accounts.
7. **Fit and safety.** Note account age (`createTime`) against follower count; very fast growth is worth a question, not an accusation. TikTok posts carry `isPaidPartnership`; list past sponsored posts if any. Public business email, when shown, is in `contact.emails`. Topics of recent posts against the brand. Note anything a brand would want to know (controversial topics, heavy competitor promotion), with links. Do not speculate about the person.
8. **Score** each creator 1–5 on reach, engagement, consistency, audience match and content fit, with one line of evidence per score.

## Output

```markdown
# Creator scorecard: {campaign}

| Creator | Followers | Median views | Reach rate | Engagement | Top audience countries | Fit | Overall |
|---|---:|---:|---:|---:|---|---|---:|
| [@handle](url) | 18.4M | | | | US 13.6%, MX 9.4% (sample 500, medium) | | /5 |

## {@handle}
- Reach: ...
- Engagement: ...
- Consistency: ...
- Audience: ...
- Content fit and notes: ...
- Contact: {public link or email from bio / link-in-bio}

## Recommendation
...

Credits used: {n}
```

## Pitfalls

- Low views ÷ followers is common on large accounts; compare creators of similar size.
- Very high engagement on a small sample can be one viral post. Use medians.
- Audience demographics come from commenters, who skew toward the most engaged fans and some regions. State the confidence field.
- Only public data. Exact follower demographics, private analytics and past brand-deal results are not available; ask the creator for their analytics screenshots.
- Do not collect personal information beyond public business contact details.
