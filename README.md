<p align="center">
  <img src="mastodon-mcp.svg" alt="mastodon-mcp-server" width="128">
</p>

# mastodon-mcp-server

A comprehensive MCP (Model Context Protocol) server for Mastodon integration. Enables AI assistants and other MCP clients to interact with Mastodon instances.

## Features

- **Timelines**: home, local, public, hashtag
- **Statuses**: post, delete, favourite, reblog, bookmark
- **Accounts**: follow, unfollow, block, mute, relationships, profile update
- **Notifications**: read, dismiss individual or all
- **Search**: accounts, statuses, hashtags
- **Trending**: tags, statuses, links
- **Lists**: create, delete, manage members
- **Media**: upload attachments
- **Polls**: vote
- **Read-only mode**: safe browsing without write access

## Installation

```bash
pip install mastodon-mcp-server
```

Or from deb package (Debian 13+ / Ubuntu 24+):

```bash
apt install mastodon-mcp-server
```

## Configuration

Copy `.env.example` to `.env` and fill in your credentials:

```bash
cp .env.example .env
```

Required variables:

| Variable | Description |
|---|---|
| `MASTODON_INSTANCE` | URL of your Mastodon instance (e.g. `https://mastodon.social`) |
| `MASTODON_ACCESS_TOKEN` | Your OAuth access token |

Optional variables:

| Variable | Default | Description |
|---|---|---|
| `READ_ONLY` | `false` | Disable all write operations |
| `MASTODON_MCP_TRANSPORT` | `stdio` | Transport: `stdio` or `streamable-http` |
| `MASTODON_MCP_HOST` | `127.0.0.1` | HTTP transport host |
| `MASTODON_MCP_PORT` | `8000` | HTTP transport port |
| `DEBUG` | not set | Enable verbose logging |

### Getting an Access Token

1. Go to your Mastodon instance → Preferences → Development → New application
2. Grant required scopes (`read`, `write`, `follow`)
3. Copy the **Your access token** value

## Usage

### stdio (default, for Claude Desktop / Claude Code)

```bash
mastodon-mcp
```

Claude Desktop config (`~/.config/claude/claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "mastodon": {
      "command": "mastodon-mcp",
      "env": {
        "MASTODON_INSTANCE": "https://mastodon.social",
        "MASTODON_ACCESS_TOKEN": "your-token-here"
      }
    }
  }
}
```

### HTTP transport

```bash
MASTODON_MCP_TRANSPORT=streamable-http mastodon-mcp
```

## Available Tools

| Tool | Description |
|---|---|
| `instance_info` | Instance details |
| `account_verify` | Own profile |
| `account_get` | Account by ID |
| `account_search` | Search accounts |
| `account_statuses` | Account's posts |
| `account_followers` / `account_following` | Social graph |
| `account_follow` / `account_unfollow` | Follow management |
| `account_block` / `account_unblock` | Block management |
| `account_mute` / `account_unmute` | Mute management |
| `account_relationships` | Relationship status |
| `account_update` | Update own profile |
| `timeline_home` | Home timeline |
| `timeline_local` | Local public timeline |
| `timeline_public` | Federated timeline |
| `timeline_hashtag` | Hashtag timeline |
| `status_get` | Get single status |
| `status_context` | Thread context |
| `status_post` | Post a status |
| `status_delete` | Delete a status |
| `status_favourite` / `status_unfavourite` | Favourite management |
| `status_reblog` / `status_unreblog` | Boost management |
| `status_bookmark` / `status_unbookmark` | Bookmark management |
| `status_favourited_by` / `status_reblogged_by` | Engagement lists |
| `notifications_get` | Get notifications |
| `notification_dismiss` | Dismiss one notification |
| `notifications_clear` | Clear all notifications |
| `search` | Search accounts/statuses/hashtags |
| `trending_tags` / `trending_statuses` / `trending_links` | Trending content |
| `favourites` | Own favourites |
| `bookmarks` | Own bookmarks |
| `lists_get` / `list_create` / `list_delete` | List management |
| `list_accounts` / `list_accounts_add` / `list_accounts_delete` | List members |
| `poll_vote` | Vote in a poll |
| `follow_requests` / `follow_request_authorize` / `follow_request_reject` | Follow requests |
| `mutes` / `blocks` | Muted/blocked accounts |
| `media_post` | Upload media |
| `directory` | Profile directory |

## License

MIT — Vítězslav Dvořák <info@vitexsoftware.cz>
