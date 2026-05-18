---
title: "Setting Up an MCP Server"
date: 2026-05-17
tags:
  - wiki
  - ai/protocol
  - ai/how-to
aliases:
  - "MCP Setup"
  - "Configure MCP"
---

# Setting Up an MCP Server

## Table of Contents
- [[#Simple Explanation]]
- [[#Prerequisites]]
- [[#Option 1 — Use a Pre-Built MCP Server]]
  - [[#Claude Desktop]]
  - [[#VS Code / GitHub Copilot]]
- [[#Option 2 — Run Any Pre-Built Server with npx]]
- [[#Popular Pre-Built Servers]]
- [[#Option 3 — Build a Custom MCP Server]]
- [[#Troubleshooting]]
- [[#Related Notes]]

---

## Simple Explanation

Setting up an MCP server means two things: **running the server program** and **telling your AI app where to find it**. The server is usually a small Node.js or Python program you either install from npm or write yourself. Your AI app (Claude Desktop, VS Code Copilot, etc.) reads a config file that points to it, and from then on the AI can use whatever tools the server exposes.

> **Analogy:** It's like plugging in a USB device — you plug it in (run the server), the OS detects it (the AI reads the config), and you can immediately use it.

---

## Prerequisites

- **Node.js** (v18+) — most pre-built MCP servers use `npx` to run
- **Your AI host app** — Claude Desktop, VS Code with GitHub Copilot, or another MCP-compatible client
- Optional: **Python 3.10+** if using Python-based MCP servers

---

## Option 1 — Use a Pre-Built MCP Server

### Claude Desktop

1. Open (or create) the config file:
   - **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
   - **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`

2. Add an `mcpServers` block. Example for the **filesystem** server:
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "C:\\Users\\YourName\\Documents"
      ]
    }
  }
}
```

3. **Restart Claude Desktop.** A hammer icon appears in the input bar when MCP servers are active.

4. Test it — ask Claude: *"List the files in my documents folder."*

---

### VS Code / GitHub Copilot

1. Open **Settings** → search for `mcp`
2. Edit `.vscode/mcp.json` in your workspace (or user settings):
```json
{
  "servers": {
    "filesystem": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "${workspaceFolder}"
      ]
    }
  }
}
```
3. Reload VS Code. Copilot Chat in **Agent mode** can now call the server's tools.

---

## Option 2 — Run Any Pre-Built Server with npx

Most official servers follow this pattern:
```bash
npx -y @modelcontextprotocol/server-<name> [args]
```

You can also run them manually first to verify they work before adding to config:
```bash
npx -y @modelcontextprotocol/server-filesystem ./my-folder
```

---

## Popular Pre-Built Servers

| Server | npm package | What it does |
|--------|-------------|--------------|
| **Filesystem** | `@modelcontextprotocol/server-filesystem` | Read/write local files in a folder |
| **GitHub** | `@modelcontextprotocol/server-github` | Search repos, read files, manage issues |
| **Brave Search** | `@modelcontextprotocol/server-brave-search` | Web search via Brave API |
| **Puppeteer** | `@modelcontextprotocol/server-puppeteer` | Control a browser, scrape pages |
| **SQLite** | `@modelcontextprotocol/server-sqlite` | Query a local SQLite database |
| **Memory** | `@modelcontextprotocol/server-memory` | Persistent key-value memory for the AI |
| **Fetch** | `@modelcontextprotocol/server-fetch` | Fetch and read web page content |

Full list: [modelcontextprotocol.io/servers](https://modelcontextprotocol.io/servers)

---

## Option 3 — Build a Custom MCP Server

For when no pre-built server does what you need.

**TypeScript (recommended):**
```bash
npm install @modelcontextprotocol/sdk
```

```typescript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

const server = new McpServer({ name: "my-server", version: "1.0.0" });

server.tool("say_hello", "Returns a greeting", {}, async () => ({
  content: [{ type: "text", text: "Hello from my MCP server!" }]
}));

const transport = new StdioServerTransport();
await server.connect(transport);
```

**Python:**
```bash
pip install mcp
```

```python
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("my-server")

@mcp.tool()
def say_hello() -> str:
    """Returns a greeting."""
    return "Hello from my MCP server!"

mcp.run()
```

---

## Troubleshooting

| Problem | Likely cause | Fix |
|---------|-------------|-----|
| Hammer icon missing in Claude Desktop | Config JSON has a syntax error | Validate JSON at jsonlint.com |
| "Server not found" error | Wrong path in `args` | Use absolute paths, not relative |
| `npx` not found | Node.js not installed | Install Node.js v18+ |
| Server connects but no tools appear | Server crashed on startup | Check Claude Desktop logs: `%APPDATA%\Claude\logs\` |
| Works in Claude, not in VS Code | Different config file location | Add to `.vscode/mcp.json`, not `claude_desktop_config.json` |

---

## Related Notes
- [[MCP Server]] — what MCP is and how it works conceptually
- [[MOC - AI]] — parent index
