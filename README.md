# 1Social plugins for Claude

Publish and schedule social media posts from Claude through [1Social](https://1social.dev). Each folder is one plugin, listed separately in Anthropic's plugin directory: one per network plus an all-networks scheduler. They share the same remote MCP server (`https://mcp.1social.dev/mcp`); each carries a skill with that network's rules.

| Plugin | Folder |
|---|---|
| Instagram by 1Social | [`instagram-by-1social`](instagram-by-1social) |
| TikTok by 1Social | [`tiktok-by-1social`](tiktok-by-1social) |
| X (Twitter) by 1Social | [`x-twitter-by-1social`](x-twitter-by-1social) |
| Threads by 1Social | [`threads-by-1social`](threads-by-1social) |
| LinkedIn by 1Social | [`linkedin-by-1social`](linkedin-by-1social) |
| Facebook by 1Social | [`facebook-by-1social`](facebook-by-1social) |
| YouTube Shorts by 1Social | [`youtube-shorts-by-1social`](youtube-shorts-by-1social) |
| Bluesky by 1Social | [`bluesky-by-1social`](bluesky-by-1social) |
| Pinterest by 1Social | [`pinterest-by-1social`](pinterest-by-1social) |
| Telegram by 1Social | [`telegram-by-1social`](telegram-by-1social) |
| Tumblr by 1Social | [`tumblr-by-1social`](tumblr-by-1social) |
| Discord by 1Social | [`discord-by-1social`](discord-by-1social) |
| Post Scheduler by 1Social | [`post-scheduler-by-1social`](post-scheduler-by-1social) |

Each plugin runs no local code: it is a `.mcp.json` pointing at the 1Social MCP server, a skill, a README and a license. See each folder's README for what data the plugin sends and where.

License: MIT.
