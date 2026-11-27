---
name: tiktok-posting
description: "Write, publish and schedule TikTok posts through the user's 1Social account, following TikTok's limits and format rules. Use when the user wants to post to TikTok, schedule TikTok content, turn text or media into a post for TikTok, or check, edit or delete what was posted there."
---

# TikTok with 1Social

1Social publishes to the user's own TikTok account and reports what actually reached it.

## Workflow

1. **Find the account.** Call `list_accounts`. It returns the connected channels with their ids, each network's live limits, the plan and the remaining monthly quota. If no TikTok account is connected, give the user the TikTok link from `connectLinks` in that result: it opens 1Social in their browser with a one-click Connect button. When they say they are done, call `list_accounts` again. If `list_accounts` reports a limit that differs from this skill, trust `list_accounts`.
2. **Draft for the network.** Write the post to the rules below. A post is a list of `segments`; each segment is `{ text, mediaIds? }`.
3. **Attach media.**
   - The user attached a file in chat → put it in `media`.
   - You have a public https link → put it in `mediaUrls`.
   - You only have a file on disk (Claude Code) → call `upload_media` and follow the upload steps its description gives, then put the returned `publicUrl` in `mediaUrls`. A localhost URL never works.
4. **Preview.** Call `preview_post` with the channel ids and the draft. It is free and publishes nothing. Fix every issue it reports and preview again.
5. **Confirm with the user.** Show the final text, the account it goes to, the media, and when it will go out. Publishing is public and some networks cannot take a post down, so wait for a clear yes.
6. **Publish or schedule.** Call `publish_post`. `scheduledAt: null` publishes now; a UTC ISO-8601 timestamp such as `2026-11-03T07:00:00Z` schedules it (in the future, at most 30 days ahead). Convert the user's local time to UTC yourself, and if they name a time without a timezone, ask which one. Set an `idempotencyKey` (a stable string of 8–128 characters for this draft) so a repeated call cannot publish twice.
7. **Report honestly.** The result is a per-network receipt. Report each network exactly as returned. `unknown` means 1Social genuinely could not tell whether the post went out — say so, never call it a success or a failure.

## TikTok rules

- Every post needs a video or photos. A caption on its own is rejected — ask the user for media before drafting.
- Photos must be JPEG. PNG is rejected by TikTok; ask for a JPEG or tell the user to convert it.
- Caption limit: 2,200 characters.
- No threads: several segments are joined into one caption.
- A published TikTok cannot be deleted through 1Social — TikTok has no delete API. Make sure the user is happy before publishing.
- TikTok may not return a link to the published video. If the receipt has no URL, tell the user to check their TikTok profile rather than saying the post failed.

## Writing tips

- Keep the caption short and front-load the hook; most viewers never expand it.
- Vertical 9:16 video is the native format.

## Posting to other networks too

The same 1Social account can publish one post to Instagram, TikTok, LinkedIn, X, Threads, Facebook Pages, YouTube Shorts, Bluesky, Pinterest, Telegram, Tumblr and Discord at once: list several channel ids from `list_accounts`. preview_post checks the draft against every selected network.

## After publishing

- **Check status:** `list_posts` (give a `postId` for one post). It includes stored engagement metrics where the network reports them.
- **Change a scheduled post:** `update_post`. Only works before the post has gone out.
- **Unschedule:** `cancel_post`. Use this, not `delete_post`, for a post that has not been published.
- **Delete a published post:** `delete_post`, only after the user confirms. Report per network what actually came down: Instagram, TikTok and YouTube posts (and Threads posts on accounts connected before 27 November 2026) stay live and only the user can remove them.
- **A delivery failed:** `retry_delivery` retries one network and spends quota again. It is refused for permanent failures such as a revoked connection — then give the user that account's `reconnectUrl` from `list_accounts`.
- **A delivery is `unknown`:** ask the user to look at the network. Only when they tell you whether the post is live, call `confirm_delivery` with their answer. Never guess, and never `retry_delivery` with `force` without asking — it may publish a second copy.
