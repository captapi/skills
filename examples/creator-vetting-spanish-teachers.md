# Example: creator-vetting

**Prompt:** "Find two Spanish-teacher creators on TikTok and vet them for a US campaign for a language app."

Real run on 29 Oct 2026. 7 calls, 13 credits.

---

# Creator scorecard: US language-app campaign

Found with `/v1/tiktok/search/users?q=spanish teacher` (1 credit), then checked the two accounts that post Spanish lessons. Scored posts: last ~25, minus posts younger than 72 hours and pinned old posts.

| Creator | Followers | Median views | Reach rate | Engagement | Consistency | Top audience countries | Overall |
|---|---:|---:|---:|---:|---:|---|---:|
| [@profe_madison](https://www.tiktok.com/@profe_madison) | 38,817 | 6,098 | 15.7% | 4.2% | 78% | US 90.9% (sample 351, low confidence) | 4/5 |
| [@latinateacherkira](https://www.tiktok.com/@latinateacherkira) | 80,184 | 6,141 | 7.7% | 9.0% | 86% | Not usable (region sample of 3) | 3/5 |

Reach rate = median views ÷ followers. Engagement = median (likes + comments) ÷ views. Consistency = share of posts within 0.5x–2x of the median.

## @profe_madison

- **Reach:** strong for her size. Median 6.1k views on 38.8k followers.
- **Engagement:** 4.2%, but comments are thin (median 3.5 per post); most engagement is likes.
- **Consistency:** 78% of posts near the median; best scored post 15.5k. A post from 29 Oct already has 149k views (too new to score, but worth watching).
- **Audience:** 90.9% US among sampled repliers, 94% English-language commenters. Fits a US campaign.
- **Content fit:** high-school Spanish teacher; bilingual-kids and "no sabo" topics.
- **Contact:** public "ugc & collabs" email in bio (left out of this example); Linktree linked.
- **Note:** the feed also returns three older viral posts (1.7M–8.5M views, 2022–2025, likely pinned). Do not use them to judge current reach.

## @latinateacherkira

- **Reach:** median 6.1k on 80.2k followers; account created Dec 2025, so growth is recent.
- **Engagement:** highest of the two (9.0%, median 20 comments).
- **Consistency:** very steady (86%); every post is a photo slideshow ("Spanish lessons with Kira", "How to say ___ in Spanish").
- **Audience:** the region sample had only 3 repliers, so the country mix is not usable. Language mix from 210 commenters: 52% English, 12% Spanish.
- **Contact:** no email in bio; Instagram and X handles listed.
- **Paid partnerships:** none flagged in the sampled posts (`isPaidPartnership: false`).

## Recommendation

Start with @profe_madison for a US-only campaign: confirmed US audience, public collab email, steady reach. @latinateacherkira has stronger engagement but an unverified audience location; ask her for TikTok analytics screenshots (audience by country) before booking.

Credits used: 13
