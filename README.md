# Zeroheight MCP Server

> Connect your AI assistant to your zeroheight design system — enabling it to read your documentation, component guidelines, design tokens, and more.

[![npm version](https://img.shields.io/npm/v/@zeroheight/mcp-server)](https://www.npmjs.com/package/@zeroheight/mcp-server)
[![Node.js](https://img.shields.io/badge/node-%3E%3D22-brightgreen)](https://nodejs.org)
[![MCP](https://img.shields.io/badge/MCP-compatible-blue)](https://modelcontextprotocol.io)
[![License: ISC](https://img.shields.io/badge/License-ISC-yellow.svg)](https://opensource.org/licenses/ISC)

---

## What is this?

This is a reference guide and setup documentation for the official [`@zeroheight/mcp-server`](https://www.npmjs.com/package/@zeroheight/mcp-server) — a [Model Context Protocol (MCP)](https://modelcontextprotocol.io) server that gives AI coding assistants (Claude, Cursor, Windsurf, etc.) direct access to your zeroheight design system.

Instead of copying and pasting design guidelines into your AI chat, the MCP server lets your AI **read your docs live** — so it generates UI that actually follows your design system.

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [Authentication](#authentication)
- [Installation](#installation)
  - [Claude Desktop](#claude-desktop)
  - [VS Code](#vs-code)
  - [Cursor](#cursor)
  - [Windsurf](#windsurf)
  - [Cline](#cline)
  - [Claude Code (CLI)](#claude-code-cli)
- [Available Tools](#available-tools)
- [Example Prompts](#example-prompts)
- [Telemetry](#telemetry)
- [Troubleshooting](#troubleshooting)
- [Changelog](#changelog)

---

## Prerequisites

- **Node.js 22+** — [Download](https://nodejs.org)
- A **zeroheight account** with at least one published styleguide
- A **Client ID** and **Access Token** from zeroheight ([how to get them](https://developers.zeroheight.com/75fe5b2ed/p/6599ef-creation))

---

## Authentication

You need two credentials from zeroheight:

| Variable | Example | Description |
|---|---|---|
| `ZEROHEIGHT_CLIENT_ID` | `zhci_abc123` | Your zeroheight OAuth client ID |
| `ZEROHEIGHT_ACCESS_TOKEN` | `zhat_abc123` | Your zeroheight access token |

Get them by following the [official zeroheight docs](https://developers.zeroheight.com/75fe5b2ed/p/6599ef-creation).

---

## Installation

### Claude Desktop

Edit `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "zeroheight": {
      "command": "npx",
      "args": ["-y", "@zeroheight/mcp-server@latest"],
      "env": {
        "ZEROHEIGHT_CLIENT_ID": "zhci_abc123",
        "ZEROHEIGHT_ACCESS_TOKEN": "zhat_abc123"
      }
    }
  }
}
```

Restart Claude Desktop after saving.

---

### VS Code

Create or edit `.vscode/mcp.json` in your project root:

```json
{
  "servers": {
    "zeroheight": {
      "command": "npx",
      "args": ["-y", "@zeroheight/mcp-server@latest"],
      "env": {
        "ZEROHEIGHT_CLIENT_ID": "zhci_abc123",
        "ZEROHEIGHT_ACCESS_TOKEN": "zhat_abc123"
      }
    }
  }
}
```

Or add it to your VS Code user settings (`settings.json`):

```json
{
  "mcp.servers": {
    "zeroheight": {
      "command": "npx",
      "args": ["-y", "@zeroheight/mcp-server@latest"],
      "env": {
        "ZEROHEIGHT_CLIENT_ID": "zhci_abc123",
        "ZEROHEIGHT_ACCESS_TOKEN": "zhat_abc123"
      }
    }
  }
}
```

---

### Cursor

Edit `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "zeroheight": {
      "command": "npx",
      "args": ["-y", "@zeroheight/mcp-server@latest"],
      "env": {
        "ZEROHEIGHT_CLIENT_ID": "zhci_abc123",
        "ZEROHEIGHT_ACCESS_TOKEN": "zhat_abc123"
      }
    }
  }
}
```

---

### Windsurf

Edit `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "zeroheight": {
      "command": "npx",
      "args": ["-y", "@zeroheight/mcp-server@latest"],
      "env": {
        "ZEROHEIGHT_CLIENT_ID": "zhci_abc123",
        "ZEROHEIGHT_ACCESS_TOKEN": "zhat_abc123"
      }
    }
  }
}
```

---

### Cline

In Cline's MCP settings, add a new server:

```json
{
  "zeroheight": {
    "command": "npx",
    "args": ["-y", "@zeroheight/mcp-server@latest"],
    "env": {
      "ZEROHEIGHT_CLIENT_ID": "zhci_abc123",
      "ZEROHEIGHT_ACCESS_TOKEN": "zhat_abc123"
    }
  }
}
```

---

### Claude Code (CLI)

Run once to add permanently:

```bash
claude mcp add zeroheight \
  -e ZEROHEIGHT_CLIENT_ID=zhci_abc123 \
  -e ZEROHEIGHT_ACCESS_TOKEN=zhat_abc123 \
  -- npx -y @zeroheight/mcp-server@latest
```

Or add to your project's `.mcp.json`:

```json
{
  "mcpServers": {
    "zeroheight": {
      "command": "npx",
      "args": ["-y", "@zeroheight/mcp-server@latest"],
      "env": {
        "ZEROHEIGHT_CLIENT_ID": "zhci_abc123",
        "ZEROHEIGHT_ACCESS_TOKEN": "zhat_abc123"
      }
    }
  }
}
```

---

## Available Tools

The MCP server exposes the following tools to AI assistants.

### Core Tools

| Tool | Description |
|---|---|
| `list-styleguides` | Lists all accessible design systems/styleguides in your zeroheight account |
| `get-styleguide-tree` | Returns the full navigation hierarchy of a styleguide (categories, pages, tabs) |
| `list-pages` | Lists all pages in a styleguide, with optional filtering by name or release version |
| `get-page` | Fetches the full content of a specific page, including usage notes and recommendations |

### Tool Details

#### `list-styleguides`

Discovers all design systems your credentials have access to. Use this first to get styleguide IDs.

```
No parameters required
```

---

#### `get-styleguide-tree`

Returns the full navigation tree of a design system.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `styleguideId` | `integer` | Yes | ID from `list-styleguides` |

---

#### `list-pages`

Explores pages within a design system, optionally filtered.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `styleguideId` | `integer` | Yes | ID from `list-styleguides` |
| `searchTerm` | `string` | No | Filter pages by name (e.g. "button", "typography") |
| `releaseId` | `integer` | No | Specific release version; defaults to latest |

---

#### `get-page`

Fetches full content from a specific design system page.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `pageId` | `string` | Yes | Page ID from `list-pages` or `get-styleguide-tree` |
| `releaseId` | `integer` | No | Specific release version; defaults to latest |

---

### Experimental Tools

> These tools are feature-flagged and may not be available in all configurations.

| Tool | Description |
|---|---|
| `list-token-sets` | Lists all accessible W3C design token sets |
| `get-tokens` | Fetches W3C design tokens from a token set |
| `lint-code` | Lints CSS/SCSS/TSX/TS/JS/JSX code against design tokens to find hardcoded values that should use tokens |

#### `lint-code` parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `code` | `string` | Yes | Source code to lint |
| `language` | `enum` | Yes | One of: `css`, `scss`, `tsx`, `ts`, `js`, `jsx` |
| `tokenSetId` | `integer` | Yes | Token set ID to validate against |
| `colorDistanceThreshold` | `number` | No | Color matching threshold 0–100 (default: 10) |

---

## Example Prompts

Here are prompts that work well once the MCP server is connected:

```
List my styleguides
```
```
Show me the navigation structure for [styleguide name]
```
```
Find pages about buttons in my design system
```
```
Get the documentation for the typography page
```
```
What are the spacing guidelines in my design system?
```
```
Build a card component that follows our design system guidelines
```
```
Show me pages about accessibility
```
```
Get documentation for the color palette page and use it to build a badge component
```

---

## Telemetry

The MCP server uses [Sentry](https://sentry.io) for error tracking to help zeroheight improve reliability.

To opt out, set the environment variable:

```json
{
  "env": {
    "ZEROHEIGHT_MCP_DISABLE_TELEMETRY": "true"
  }
}
```

No design content or page data is sent — only error/crash diagnostics.

---

## Troubleshooting

**"There are no accessible styleguides"**
- Check that `ZEROHEIGHT_CLIENT_ID` and `ZEROHEIGHT_ACCESS_TOKEN` are set correctly
- Make sure your account has at least one published styleguide
- Verify the credentials haven't expired

**Tool not appearing in the AI assistant**
- Restart the application after editing the config file
- Check config file syntax (valid JSON required)
- Confirm Node.js 22+ is installed: `node --version`

**"Could not find the page"**
- Use `list-pages` first to get valid page IDs
- Ensure the page is published (draft pages are not returned)

**npx is slow on first run**
- This is expected — `npx` downloads the package on first use
- Subsequent runs use the npm cache and are faster

---

## Changelog

See the full history on [npm](https://www.npmjs.com/package/@zeroheight/mcp-server?activeTab=versions).

| Version | Date | Notes |
|---|---|---|
| 2.2.1 | 10 Mar 2026 | Temporarily disabled embedded image resources in `get-page` responses |
| 2.2.0 | 26 Feb 2026 | Images as embedded resources in `get-page`; removed JSON response format |
| 2.1.1 | 16 Feb 2026 | Bug fixes |
| 2.1.0 | 9 Dec 2025 | Beta feature |
| 2.0.0 | 28 Nov 2025 | Removed resources |
| 1.3.0 | 12 Nov 2025 | Only return published pages, categories, and navigation |
| 1.2.0 | 9 Sep 2025 | Reduced token usage for `get-styleguide-tree` |
| 1.1.0 | 20 Aug 2025 | Sentry error tracking |
| 1.0.1 | 15 Aug 2025 | Fix startup via `npx` |
| 1.0.0 | 15 Aug 2025 | Initial release |

---

## Resources

- [zeroheight Help Centre — MCP Server](https://help.zeroheight.com/hc/en-us/articles/39914754674843-Using-the-zeroheight-MCP-server)
- [zeroheight Developer Docs](https://developers.zeroheight.com)
- [Model Context Protocol](https://modelcontextprotocol.io)
- [npm package — @zeroheight/mcp-server](https://www.npmjs.com/package/@zeroheight/mcp-server)

---

*This repo documents the official `@zeroheight/mcp-server` package published by [zeroheight](https://zeroheight.com).*
