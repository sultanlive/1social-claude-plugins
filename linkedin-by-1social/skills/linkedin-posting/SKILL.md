---
name: linkedin-posting
description: "Write, publish and schedule LinkedIn posts through the user's 1Social account, following LinkedIn's limits and format rules. Use when the user wants to post to LinkedIn, schedule LinkedIn content, turn text or media into a post for LinkedIn, or check, edit or delete what was posted there."
---

# LinkedIn with 1Social

1Social publishes to the user's own LinkedIn account and reports what actually reached it.

## Workflow

1. **Find the account.** Call `list_accounts`. It returns the connected channels with their ids, each network's live limits, the plan and the remaining monthly quota. If no LinkedIn account is connected, tell the user to connect one at https://1social.dev/app — connecting needs a browser and cannot be done from chat. If `list_accounts` reports a limit that differs from this skill, trust `list_accounts`.
2. **Draft for the network.** Write the post to the rules below. A post is a list of `segments`; each segment is `{ text, mediaIds? }`.
3. **Attach media.**
   - The user attached a file in chat → pass it in `media`.
   - You have a public https link → pass it in `mediaUrls`.
   - You only have a local file (Claude Code) → call `upload_media` with the file name, MIME type and size, PUT the bytes to the returned `uploadUrl` with `uploadHeaders` exactly as given (for example with `curl -X PUT --data-binary @file`), then pass the returned `publicUrl` in `mediaUrls`. Do it straight away — the upload URL is short-lived. A localhost URL never works.
4. **Preview.** Call `preview_post` with the channel ids and the draft. It is free and publishes nothing. Fix every issue it reports and preview again.
5. **Confirm with the user.** Show the final text, the account it goes to, the media, and when it will go out. Publishing is public and some networks cannot take a post down, so wait for a clear yes.
6. **Publish or schedule.** Call `publish_post`. `scheduledAt: null` publishes now; a UTC ISO-8601 timestamp such as `2026-11-03T07:00:00Z` schedules it (in the future, at most 30 days ahead). Convert the user's local time to UTC yourself, and if they name a time without a timezone, ask which one. Pass an `idempotencyKey` (a stable string of 8–128 characters for this draft) so a repeated call cannot publish twice.
7. **Report honestly.** The result is a per-network receipt. Report each network exactly as returned. `unknown` means 1Social genuinely could not tell whether the post went out — say so, never call it a success or a failure.

## LinkedIn rules

- Limit: 3,000 characters.
- No threads: several segments are joined into one post, which 3,000 characters usually fits.
- Text-only posts work; images are optional.
- Which profile or company page is used depends on what the user connected at 1social.dev — check the account name from list_accounts.
- Deleting works: delete_post removes the post from LinkedIn.

## Writing tips

- The first two lines show before "see more" — open with the insight, not a greeting.
- Short paragraphs with blank lines between them read better in the feed.
- Three hashtags or fewer.

## Posting to other networks too

The same 1Social account can publish one post to Instagram, TikTok, LinkedIn, X, Threads, Facebook Pages, YouTube Shorts and Bluesky at once: pass several channel ids from `list_accounts`. preview_post checks the draft against every selected network.

## After publishing

- **Check status:** `list_posts` (pass `postId` for one post). It includes stored engagement metrics where the network reports them.
- **Change a scheduled post:** `update_post`. Only works before the post has gone out.
- **Unschedule:** `cancel_post`. Use this, not `delete_post`, for a post that has not been published.
- **Delete a published post:** `delete_post`, only after the user confirms. Report per network what actually came down: Instagram, TikTok, Threads and YouTube posts stay live and only the user can remove them.
- **A delivery failed:** `retry_delivery` retries one network and spends quota again. It is refused for permanent failures such as a revoked connection — then the user must reconnect at https://1social.dev/app.
- **A delivery is `unknown`:** ask the user to look at the network. Only when they tell you whether the post is live, call `confirm_delivery` with their answer. Never guess, and never `retry_delivery` with `force` without asking — it may publish a second copy.
