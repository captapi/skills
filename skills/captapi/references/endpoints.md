# Captapi endpoints used by these skills

Costs are from https://captapi.com/llms-endpoints.txt (checked 29 Oct 2026). "~N (x/result)" means the price scales with results returned. Failed calls and empty results cost 0. `cache=true` hits cost 0.

MCP tool name = path without `/v1/`, with `/` and `-` turned into `_` (for example `youtube_channel_videos`). Ad-library tools are named `facebook_ad_library_*`, `google_ad_library_*`, `linkedin_ad_library_*`, `tiktok_ad_library_*`.

## Account (free)

| Endpoint | Credits | Use |
|---|---|---|
| GET /v1/account/balance | 0 | Remaining credits |
| GET /v1/account/request-history | 0 | Recent calls with credits charged |

## YouTube

| Endpoint | Credits | Use |
|---|---|---|
| GET /v1/youtube/channel-details | 1 | Channel stats |
| GET /v1/youtube/channel-videos | 2 | Uploads, `limit` up to 200, cursor |
| GET /v1/youtube/channel-shorts | 2 | Shorts, cursor |
| GET /v1/youtube/video-details | 1 | One video's metadata and stats |
| GET /v1/youtube/comments | 2 | Comments, `limit` up to 500, cursor |
| GET /v1/youtube/comment-replies | 2 | Replies under one comment |
| GET /v1/youtube/transcript | 1 | Published captions with timestamps (not speech-to-text) |
| GET /v1/youtube/summarize | 3 | AI summary from captions |
| GET /v1/youtube/audio-transcript | 2 per minute of audio | Speech-to-text when captions are missing (ask first) |
| GET /v1/youtube/search | 2 | Search with filters, cursor |

## TikTok

| Endpoint | Credits | Use |
|---|---|---|
| GET /v1/tiktok/channel-details | 1 | Profile stats, bio link, verification |
| GET /v1/tiktok/channel-posts | 2 | Latest videos, `limit` up to 200, cursor |
| GET /v1/tiktok/video-details | 1 | One video's stats |
| GET /v1/tiktok/comments | 2 | Comments |
| GET /v1/tiktok/transcript | 2 | Timestamped transcript |
| GET /v1/tiktok/summarize | 4 | AI summary |
| GET /v1/tiktok/profile-region | 2 | Likely country and language of a creator |
| GET /v1/tiktok/audience-demographics | 3 (12 videos), 5 (30), 8 (60) | Audience country and language mix from commenters (a sample, not a follower census) |
| GET /v1/tiktok/search-suggestions | 2 | Autocomplete phrases people search |
| GET /v1/tiktok/top-search | 2 | Keyword search |
| GET /v1/tiktok/trending-feed | 2 | Trending videos |
| GET /v1/tiktok/popular-hashtags | 2 | Related and popular hashtags |
| GET /v1/tiktok/popular-creators | 2 | Creator discovery |

## Instagram

| Endpoint | Credits | Use |
|---|---|---|
| GET /v1/instagram/channel-details | 1 | Profile stats |
| GET /v1/instagram/channel-reels | ~0.3/result | Latest Reels, cursor |
| GET /v1/instagram/channel-posts | ~0.3/result | Latest posts |
| GET /v1/instagram/details | 1 | One post or Reel |
| GET /v1/instagram/comments | ~0.9/result | Comments (expensive; ask before large pulls) |
| GET /v1/instagram/transcript | 2 | Reel transcript |
| GET /v1/instagram/summarize | 4 | Reel summary |
| GET /v1/instagram/reels-search | 2 | Keyword Reels search |
| GET /v1/instagram/hashtag-search | 2 | Hashtag feed |

## Reddit

| Endpoint | Credits | Use |
|---|---|---|
| GET /v1/reddit/search | 2 | Site-wide post search, `sort`, `timeframe`, cursor |
| GET /v1/reddit/subreddit-search | 2 | Search inside one subreddit |
| GET /v1/reddit/subreddit-posts | 2 | List a subreddit |
| GET /v1/reddit/post-comments | 2 | Post body plus flat comment thread |
| GET /v1/reddit/subreddit-details | 1 | Subreddit card and rules |

## X/Twitter, LinkedIn, Threads, Facebook

| Endpoint | Credits | Use |
|---|---|---|
| GET /v1/twitter/user-tweets | 2 | Most popular recent tweets (~100 cap, not chronological) |
| GET /v1/twitter/profile | 1 | Profile |
| GET /v1/linkedin/company-posts | 1 | About 10 recent company posts |
| GET /v1/linkedin/company | 2 | Company page |
| GET /v1/threads/user-posts | 2 | Recent posts with engagement |
| GET /v1/threads/search | 2 | Keyword search |
| GET /v1/facebook/profile-reels | 2 | Page Reels with views |
| GET /v1/facebook/comments | 2 | Post comments |

## Ad libraries

| Endpoint | Credits | Use |
|---|---|---|
| GET /v1/ad-library/facebook/search-companies | 2 | Brand name to Meta `pageId` |
| GET /v1/ad-library/facebook/company-ads | 2 | Ads for one page (`status` ACTIVE/INACTIVE/ALL, `country`) |
| GET /v1/ad-library/facebook/search | 2 | Keyword search of Meta ads |
| GET /v1/ad-library/google/advertiser-search | 1 | Brand to Google advertiser |
| GET /v1/ad-library/google/company-ads | 2 | Google ads for an advertiser |
| GET /v1/ad-library/linkedin/search-ads | 2 | LinkedIn ads by company or keyword |
| GET /v1/ad-library/tiktok/search | 2 (5 via fallback) | TikTok ads (EU/EEA, UK and Switzerland only) |

## Link-in-bio

| Endpoint | Credits | Use |
|---|---|---|
| GET /v1/linktree/page | 1 | Links, socials and public email from a Linktree |

## Utilities

| Endpoint | Credits | Use |
|---|---|---|
| GET /v1/analytics/compare | 1 per URL | Unified metrics for up to 10 post URLs across platforms |
| POST /v1/batch | sum of items | Up to 20 GET calls in one request |
