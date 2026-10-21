# TikTok by 1Social

Publish and schedule TikTok videos and photo posts from Claude. With this plugin Claude can write TikTok posts that TikTok will actually accept, publish them now or schedule them up to 30 days ahead, and see what reached your account. The bundled skill teaches Claude TikTok's rules — length limits, required media, formats — so problems are caught before publishing, not after.

It connects Claude to [1Social](https://1social.dev), a publishing service for social media accounts you own.

## What you get

- **The 1Social MCP server** (`https://mcp.1social.dev/mcp`): tools to list your connected accounts, preview a post against each network's limits, publish or schedule it, edit or cancel a scheduled post, retry a failed delivery, and read per-network delivery receipts.
- **A skill** that tells Claude how to use those tools for TikTok: the network's limits and format rules, and to confirm with you before anything is published.

## Requirements

- A 1Social account. New accounts start with a free trial; publishing after the trial needs a paid plan — see [pricing](https://1social.dev/pricing).
- Your TikTok account connected at [1social.dev](https://1social.dev/app). Connecting an account happens in the browser, not in chat.

## Setup

1. Install the plugin.
2. The first time Claude uses a 1Social tool, sign in to 1Social in the browser window that opens (OAuth 2.1 — no API keys to copy).
3. Ask Claude something like "Schedule this as a TikTok post for Friday 10am".

## What data leaves your machine

This plugin runs no local code. Everything goes through the remote MCP server at `https://mcp.1social.dev/mcp`, operated by 1Social:

- the text and media of posts you ask Claude to preview or publish, and the times you schedule them for;
- media files are stored in 1Social's storage (Cloudflare R2) and fetched from there by the networks when the post is published;
- published posts go to the social networks you connected, directly or through a publishing provider 1Social contracts.

Nothing is published without a tool call, and the skill asks Claude to confirm with you first. 1Social does not use your content to train AI models. Details: [privacy policy](https://1social.dev/legal/privacy), [terms](https://1social.dev/legal/terms).

## Support

[1social.dev/support](https://1social.dev/support) · support@1social.dev · more about this integration: [1social.dev/post-to/tiktok](https://1social.dev/post-to/tiktok)

## License

MIT
