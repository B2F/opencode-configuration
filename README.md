# opencode Workspace Configuration

This repository contains a local **opencode** workspace setup and a curated set of custom agent definitions.

If you use opencode for AI-assisted development, this repo gives you a reproducible starting point for:
- managing providers and permissions, and
- using specialized subagents for writing, review, architecture, research, and product work.

## Features

- 26 custom agent prompt files in `agents/`
- Permission defaults configured for common tools (`read`, `glob`, `bash`, etc.)
- Preconfigured Brave Search MCP + subagent
- Preconfigured DuckDuckGo Search MCP server
- Custom DuckDuckGo research subagent (`agents/duckduckgo-search.md`)
- Six research workflow subagents adapted from web-forager skill patterns:
  - `agents/web-research.md`
  - `agents/fact-check.md`
  - `agents/news-monitor.md`
  - `agents/competitive-intel.md`
  - `agents/tech-advisor.md`
  - `agents/scholarly-fact-check.md`
- Preconfigured LM Studio model sample

## MCP servers

This workspace is configured with ten MCP integrations:

- `brave-search`: Brave Search MCP server
- `duckduckgo-search`: DuckDuckGo MCP server (`nickclyde/duckduckgo-mcp-server`)
- `pubmed`: PubMed MCP server (executable name: `pubmed-mcp-server`)
- `crossref`: Crossref MCP server (executable name: `crossref-mcp`)
- `semantic-scholar`: Semantic Scholar MCP server (executable name: `semanticscholar-mcp`)
- `playwright`: Playwright MCP server (executable name: `playwright-mcp`)
- `browserbase`: Browserbase MCP server (executable name: `mcp-server-browserbase`)
- `chrome`: Chrome DevTools MCP server (run via `npx -y chrome-devtools-mcp`)
- `youtube`: YouTube Data API MCP server (executable name: `mcp-youtube`)
- `youtube-transcript`: YouTube transcript MCP server (executable name: `youtube-mcp`)

Check if MCP servers are active:

`opencode mcp list`

### MCP dependency install map

This workspace relies on these MCP package dependencies:

- `brave-search` -> `@brave/brave-search-mcp-server` (run via `npx`)
- `duckduckgo-search` -> `duckduckgo-mcp-server` (Python package/executable on `PATH`)
- `pubmed` -> `@cyanheads/pubmed-mcp-server`
- `crossref` -> `@botanicastudios/crossref-mcp`
- `semantic-scholar` -> `@xbghc/semanticscholar-mcp`
- `playwright` -> `@playwright/mcp`
- `browserbase` -> `@browserbasehq/mcp`
- `chrome` -> `chrome-devtools-mcp` (run via `npx`)
- `youtube` -> `@kirbah/mcp-youtube`
- `youtube-transcript` -> `@jcodesmore/youtube-mcp`

Example install commands:

```bash
# npm-based MCP servers
npm install -g @cyanheads/pubmed-mcp-server @botanicastudios/crossref-mcp @xbghc/semanticscholar-mcp
npm install -g @playwright/mcp @browserbasehq/mcp
npm install -g @kirbah/mcp-youtube @jcodesmore/youtube-mcp

# Brave is executed via npx from opencode.json (no global install required)
# Chrome DevTools MCP is executed via npx from opencode.json (no global install required)
# DuckDuckGo server should expose `duckduckgo-mcp-server` on PATH
```

Browserbase requires these environment variables:

- `BROWSERBASE_API_KEY`
- `BROWSERBASE_PROJECT_ID`

YouTube Data API MCP requires:

- `YOUTUBE_API_KEY`

## YouTube research subagent

- `agents/youtube-research.md`: combines official YouTube Data API metadata (`youtube` MCP)
  with transcript analysis (`youtube-transcript` MCP).

### DuckDuckGo MCP local install notes

The DuckDuckGo server is installed locally in a Python virtual environment and exposed on `PATH` via a symlink:

- venv: `~/.local/share/opencode/venvs/duckduckgo-mcp`
- executable symlink: `~/.local/bin/duckduckgo-mcp-server`

The OpenCode MCP entry is stored in user config:

- `~/.config/opencode/opencode.json`

## Research subagent routing policy

These research subagents share a common policy:

- Brave is default only for fact-critical or quality-critical tasks.
- DuckDuckGo is default for exploratory scans.
- Fetch order is `duckduckgo-search_fetch_content` first, `webfetch` fallback.
- Every non-trivial factual claim should include at least one source link.

Skill-specific intent:

- `web-research`: balanced synthesis
- `fact-check`: direct claim verification
- `news-monitor`: fast brief news updates
- `competitive-intel`: comparison and market deltas
- `tech-advisor`: technical option recommendations
- `scholarly-fact-check`: stronger scientific/medical evidence checks

## Scholarly connectors

`scholarly-fact-check` works with Brave + DuckDuckGo, and this workspace now includes connector
entries for PubMed, Crossref, and Semantic Scholar in `opencode.json`.

These three entries assume the MCP server executables are on your `PATH`:

- `pubmed-mcp-server`
- `crossref-mcp`
- `semanticscholar-mcp`

Installed package mapping used in this workspace:

- PubMed: `@cyanheads/pubmed-mcp-server`
- Crossref: `@botanicastudios/crossref-mcp`
- Semantic Scholar: `@xbghc/semanticscholar-mcp`

If your chosen MCP implementations use different command names, update each `mcp.*.command`
entry accordingly.

## Project structure

```text
.
├── agents/             # Custom subagent definitions (Markdown + frontmatter)
├── opencode.json       # Workspace config (permissions/providers/models)
```

## License
MIT
