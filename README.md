<p align="center"><img src="assets/logo.png" width="96" height="96" alt="Outcrawl"></p>

# Outcrawl MCP

Give your AI agent the web. Outcrawl is web infrastructure for AI agents: connect it once, then ask in plain words. Your agent reads any page as clean markdown or JSON, crawls a whole site, searches the web with the results already read, watches pages for changes, and hands a multi-step task (a login, a form, a flow across pages) to an agent working in a real browser.

- Server: `https://api.outcrawl.ai/mcp` (streamable HTTP)
- Sign-in: OAuth in your browser on first use. No key to copy, nothing to paste into a config file.
- 30 tools: scrape, crawl, search, agent runs, monitors, profiles, recorded sessions, usage, credits and workspace rules. [Tool reference](https://outcrawl.ai/docs/mcp/tools)

The server is hosted by Outcrawl. There is no local package to install or run. You pick one workspace when you sign in, every call spends that workspace's credits, and every result reports what it cost. The Free plan has 1,000 credits a month; a page load is 1 credit. [Pricing](https://outcrawl.ai/pricing?utm_source=github&utm_medium=directory&utm_campaign=mcp)

## Connect your agent

### Claude Code

```bash
claude mcp add --transport http outcrawl https://api.outcrawl.ai/mcp
```

The first tool call opens a browser to sign in and pick a workspace.

Or install the plugin, which adds the server and a skill that teaches Claude which tool to reach for:

```text
/plugin marketplace add outcrawlai/outcrawl-mcp
/plugin install outcrawl@outcrawl
```

### Cursor

[![Add to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/en/install-mcp?name=outcrawl&config=eyJ1cmwiOiJodHRwczovL2FwaS5vdXRjcmF3bC5haS9tY3AifQ==)

Or add it in Cursor's MCP settings (`~/.cursor/mcp.json`):

```json
{ "mcpServers": { "outcrawl": { "url": "https://api.outcrawl.ai/mcp" } } }
```

### Codex

```bash
codex mcp add outcrawl --url https://api.outcrawl.ai/mcp
codex mcp login outcrawl
```

Or in `~/.codex/config.toml`:

```toml
[mcp_servers.outcrawl]
url = "https://api.outcrawl.ai/mcp"
```

### Gemini CLI

```bash
gemini extensions install https://github.com/outcrawlai/outcrawl-mcp
```

### Any other MCP client

Give the client the URL `https://api.outcrawl.ai/mcp` and choose OAuth. It registers itself and signs you in through your browser on first use:

```json
{ "mcpServers": { "outcrawl": { "url": "https://api.outcrawl.ai/mcp" } } }
```

<details>
<summary>A client with no OAuth support</summary>

Create an API key in the console at https://outcrawl.ai/console/keys and send it as a header:

```json
{
  "mcpServers": {
    "outcrawl": {
      "url": "https://api.outcrawl.ai/mcp",
      "headers": { "Authorization": "Bearer ock_YOUR_KEY_ID.YOUR_SECRET" }
    }
  }
}
```

This is the one place an API key belongs in an MCP config; every client above signs in instead.

</details>

## Tools

| Group | Tools | What they do |
| --- | --- | --- |
| Read the web | `outcrawl_scrape`, `outcrawl_crawl`, `outcrawl_search` | One known URL to markdown, HTML, links, a screenshot or JSON; a whole site from a seed URL; a web search with every hit read in full |
| Agents | `outcrawl_agent` and 9 more | Give a task to an agent in a real browser under caps you set, follow its steps, read its records as they land, cancel it, hand its browser to a person, pass it a file, list its downloads |
| Monitors | `outcrawl_monitor_create`, `outcrawl_monitor_list`, `outcrawl_monitor_delete` | Watch a URL on a schedule and notify Discord, Slack or a webhook when it really changes |
| Profiles | `outcrawl_profile_create`, `outcrawl_profile_list`, `outcrawl_profile_get`, `outcrawl_profile_delete` | Keep cookies and site storage so a login survives across runs, optionally pinned to a country, region and city |
| Sessions | `outcrawl_sessions_list`, `outcrawl_sessions_get`, `outcrawl_session_network`, `outcrawl_session_request`, `outcrawl_session_downloads`, `outcrawl_session_delete` | Every session is recorded: its replay URL, network and console digest, any request in full, its downloads |
| Account | `outcrawl_usage`, `outcrawl_credits`, `outcrawl_rules_get`, `outcrawl_rules_set` | Usage over a date range, the credit pool, and workspace rules written in plain sentences |

Pick the cheapest tool that answers: a scrape to read a page you know, a search to find pages, a crawl to cover a site, and an agent only when the answer is behind a login, a form or a multi-step flow. An agent run costs far more than a scrape for the same text.

## Try it

> Read https://outcrawl.ai/pricing and list the plans with their prices.

> Search for the latest Model Context Protocol changes and summarize the top three.

> Crawl the docs at https://outcrawl.ai/docs, at most 20 pages, and list the page titles.

> Watch https://example.com/changelog every hour and post to my Slack webhook when it changes.

## What is in this repo

| File | For |
| --- | --- |
| `.claude-plugin/` and `.mcp.json` | Claude Code plugin and marketplace |
| `plugin.json` and `mcp.json` | [Agent Plugins](https://agent-plugins.org) package (Cursor, ChatGPT, Codex) |
| `.cursor-plugin/plugin.json` | Cursor Marketplace |
| `gemini-extension.json` | Gemini CLI extension |
| `server.json` | The [official MCP registry](https://registry.modelcontextprotocol.io/v0/servers?search=io.github.outcrawlai/outcrawl) entry, `io.github.outcrawlai/outcrawl` |
| `skills/outcrawl/SKILL.md` | The skill every package ships: which tool answers which question, and how to keep spend down |

This repo only packages the connection; the server is hosted by Outcrawl.

## Links

- Docs: https://outcrawl.ai/docs/mcp/setup
- Product: https://outcrawl.ai/?utm_source=github&utm_medium=directory&utm_campaign=mcp
- Support: hi@outcrawl.ai
- Privacy: https://outcrawl.ai/legal/privacy
- Terms: https://outcrawl.ai/legal/terms

MIT licensed.
