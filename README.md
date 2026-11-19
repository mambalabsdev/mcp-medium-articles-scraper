# Medium Articles Scraper MCP Server

[![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-medium-articles-scraper%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-medium-articles-scraper&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-medium-articles-scraper)](https://www.npmjs.com/package/@mambalabsdev/mcp-medium-articles-scraper) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-medium-articles-scraper)](https://www.npmjs.com/package/@mambalabsdev/mcp-medium-articles-scraper) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-medium-articles-scraper)](https://github.com/mambalabsdev/mcp-medium-articles-scraper/blob/main/LICENSE)

An MCP server for the Mamba Labs Medium Articles Scraper actor on Apify. Medium stories as rows.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Takes Medium tags, authors, publications, or story links and returns one row per story with title, link, author, publication, publish date, reading time, claps, responses, member-only flag, and tags, optionally the story text, plus a free summary row per source.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-medium-articles-scraper": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-medium-articles-scraper"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `get_medium_articles` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Get the 50 newest Medium stories tagged python with their claps."
- "Which stories did the Netflix Technology Blog publish on Medium since October 1, and which got the most claps?"

## Tool

`get_medium_articles`. One tool. Example arguments:

```json
{
  "tags": [
    "python"
  ],
  "max_stories_per_source": 10
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `tags` | array | no | Medium tags such as python or data-engineering, or tag links such as https://medium.com/tag/python. |
| `authors` | array | no | Medium usernames (netflixtechblog or @netflixtechblog) or profile links (https://medium.com/@netflixtechblog, https://name.medium.com). |
| `publications` | array | no | Publication slugs (better-programming), links (https://medium.com/better-programming), or the publication's own domain (netflixtechblog.com). |
| `story_urls` | array | no | Links to single Medium stories, on medium.com or a publication's own domain. The story ID at the end of the link is what is read. |
| `max_stories_per_source` | number | no | Stories to read from each tag, author, or publication. Default 25, maximum 2000. |
| `since_date` | string | no | Optional. A date such as 2026-11-01, or a number of days back such as 7. Older stories are skipped and not charged. |
| `include_text` | boolean | no | yes to add each story's body as plain text, billed as a story with text. Member-only stories return the preview a logged-out reader sees, marked incomplete. Default no. |
| `max_charge_usd` | number | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `story-row`, one story | $0.003 | $0.00285 | $0.0027 | $0.00255 |
| `story-text-row`, one story with its text | $0.005 | $0.00475 | $0.0045 | $0.00425 |
| `apify-actor-start`, per run start | $0.005 | $0.005 | $0.005 | $0.005 |

Source summaries, missing names, blocked reads, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/medium-articles-scraper](https://apify.com/mambalabs/medium-articles-scraper)

## License

MIT. See [LICENSE](LICENSE).
