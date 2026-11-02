---
name: outcrawl
description: Read, search and act on the web through Outcrawl's MCP tools. Use when the user asks to fetch or read a web page, extract data from a site, crawl a site, search the web, watch a page for changes, or get something done on a website that needs a login, a form or several steps.
---

# Outcrawl

Outcrawl is web infrastructure for AI agents. The `outcrawl` MCP server gives you 30 tools on the user's own account. The user signed in through the browser and picked one workspace; every call spends that workspace's credits, and every result carries its own `usage`, so you can tell the user what a call cost.

## Pick the cheapest tool that answers

1. **You have the URL and only need to read it**: `outcrawl_scrape`. One page render, the cheapest tool. Ask for `formats` you need (markdown by default, or `links`, a screenshot, or JSON against a `schema` you pass).
2. **You need to find pages**: `outcrawl_search`. Without `scrape` it is a cheap pass-through that returns hits; set `scrape: true` only when you need the page bodies, because each hit is then a render.
3. **You need many pages from one site**: `outcrawl_crawl`. `limit` decides the bill, so set it to what the user asked for, and narrow with `includePaths` and `maxDepth`. It returns once, when the whole crawl is done.
4. **The answer is behind a decision** (a login, a form, a search box, a page that only shows content after clicking): `outcrawl_agent`. Only then. An agent run is a model in a loop with a browser attached and costs orders of magnitude more than a scrape for the same text.

If a scrape returns what the user needs, stop there. Never start an agent run to read a page you could scrape.

## Running an agent

- `outcrawl_agent` returns a run id in milliseconds with status `queued` or `running`. That is a success, not a failure: the work happens afterwards.
- Set `caps.budget` (credits, as a decimal string such as `"2.00"`) to what the task is worth, and tell the user the ceiling before you submit.
- Follow it with `outcrawl_agent_status` (the answer once it has one), `outcrawl_agent_results` (records as they land, with a cursor) and `outcrawl_agent_events` (what it is doing). Runs can take a long time. Never submit a second run because the first has not answered yet.
- If the run is `paused` with a question only the user can answer, ask the user and reply with `outcrawl_agent_answer`.
- `outcrawl_agent_control` hands the live browser to a person (for a step they must do themselves) and back.
- Stop a run you no longer need with `outcrawl_agent_cancel`. What it already found stays readable.
- Share the run's replay URL with the user so they can see what happened.

## Never

- Never make a purchase, a payment, a booking or any other irreversible action on a site unless the user asked for that exact action in this conversation, and confirm it with them first. Workspace rules (`outcrawl_rules_get`) can require confirmation and block spend; respect them and do not replace them with `rules` on a run to get around one.
- Never try to change browser settings, fingerprints or exit IPs. They are assigned by Outcrawl and are not configurable. Geography is the one exception: pass an exit such as `outcrawl://us/texas/austin` where a tool accepts `exit`.
- Never enter credentials the user has not given you for that site.

## Keeping a login

`outcrawl_profile_create` makes a profile that keeps cookies and site storage. Pass its label as `profile` to scrapes, crawls and agent runs so a login made once survives later calls. `outcrawl_profile_delete` removes it with its stored data; ask first.

## Watching a page

`outcrawl_monitor_create` checks a URL on a cron schedule and notifies Discord, Slack or a webhook when the page really changes. `outcrawl_monitor_list` shows them; `outcrawl_monitor_delete` removes one and its stored snapshots (ask first).

## Looking back

- `outcrawl_sessions_list` and `outcrawl_sessions_get`: past sessions with their replay URL, duration, pages and charges.
- `outcrawl_session_network` and `outcrawl_session_request`: why a page failed (blocked or failed requests, 4xx and 5xx, console errors).
- `outcrawl_session_downloads` and `outcrawl_agent_downloads`: files a browser downloaded.
- `outcrawl_session_delete` ends a live browser session and its billing.

## Spend

- `outcrawl_credits` shows what is left in the credit pool; `outcrawl_usage` shows usage over a date range by hour, day, month or capability.
- Ask before destructive calls: `outcrawl_monitor_delete`, `outcrawl_profile_delete`, `outcrawl_session_delete`, `outcrawl_agent_cancel`, `outcrawl_rules_set`.

Docs: https://outcrawl.ai/docs/mcp/setup
