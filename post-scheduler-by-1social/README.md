# Post Scheduler by 1Social

Publish and schedule one post to Instagram, TikTok, LinkedIn, X, Threads, Facebook, YouTube Shorts, Bluesky, Pinterest, Telegram and Tumblr from Claude. With this plugin Claude can write one post, adapt it to each network's limits, and publish it now or schedule it to Instagram, TikTok, LinkedIn, X (Twitter), Threads, Facebook Pages, YouTube Shorts, Bluesky, Pinterest, Telegram and Tumblr at once. Plan a content calendar in chat and schedule every post in it.

It connects Claude to [1Social](https://1social.dev), a publishing service for social media accounts you own.

## What you get

- **The 1Social MCP server** (`https://mcp.1social.dev/mcp`): tools to list your connected accounts, preview a post against each network's limits, publish or schedule it, edit or cancel a scheduled post, retry a failed delivery, and read per-network delivery receipts.
- **A skill** that tells Claude how to use those tools across networks: which network needs what, how to adapt one post to many, and to confirm with you before anything is published.

## Requirements

- A 1Social account. New accounts start with a free trial; publishing after the trial needs a paid plan — see [pricing](https://1social.dev/pricing).
- Your social accounts connected at [1social.dev](https://1social.dev/app). Connecting an account happens in the browser, not in chat.

## Setup

1. Install the plugin.
2. The first time Claude uses a 1Social tool, sign in to 1Social in the browser window that opens (OAuth 2.1 — no API keys to copy).
3. Ask Claude something like "Post this announcement to LinkedIn, X and Bluesky tomorrow at 9am".

## What data leaves your machine

This plugin runs no local code. Everything goes through the remote MCP server at `https://mcp.1social.dev/mcp`, operated by 1Social:

- the text and media of posts you ask Claude to preview or publish, and the times you schedule them for;
- media files are stored in 1Social's storage (Cloudflare R2) and fetched from there by the networks when the post is published;
- published posts go to the social networks you connected, directly or through a publishing provider 1Social contracts.

Nothing is published without a tool call, and the skill asks Claude to confirm with you first. 1Social does not use your content to train AI models. Details: [privacy policy](https://1social.dev/legal/privacy), [terms](https://1social.dev/legal/terms).

## Support

[1social.dev/support](https://1social.dev/support) · support@1social.dev · more about this integration: [1social.dev/mcp](https://1social.dev/mcp)

## License

MIT
