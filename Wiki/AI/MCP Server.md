---
title: "MCP Server"
date: 2026-05-17
tags:
  - wiki
  - ai/protocol
  - ai/agent
aliases:
  - "Model Context Protocol Server"
  - "MCP"
---

# MCP Server

## Table of Contents
- [[#Simple Explanation]]
- [[#What Problem Does It Solve?]]
- [[#How It Works]]
- [[#Key Terms]]
- [[#In Depth]]
  - [[#Architecture / Structure]]
  - [[#Transports]]
  - [[#How It's Used in Practice]]
  - [[#Common Gotchas / Misconceptions]]
- [[#Quick Reference]]
- [[#Related Notes]]
- [[#References]]

---

## Simple Explanation

An MCP server is a **middleman that lets an AI talk to other programs and data sources** in a standardized way.

Think of it like a **USB standard for AI**. Before USB, every device used a different plug — printers, keyboards, and mice all needed different ports. USB created one universal connector so any device could plug into any computer. MCP does the same thing for AI: instead of every AI tool being custom-wired to every data source, MCP creates one universal "plug" so an AI assistant can connect to your files, your calendar, a database, a web browser — anything — without special one-off code.

> **Analogy:** Imagine a contractor (the AI) who can only do jobs if they have the right tools. MCP is the tool belt — a standardized way to hand the contractor whatever tool they need (a hammer, a drill, a saw) on demand.

---

## What Problem Does It Solve?

Before MCP, connecting an AI to external tools was messy:
- Every integration was **custom-built** — if you wanted Claude to read your files AND query a database, someone had to write two completely separate connectors.
- Those connectors were **not reusable** — a connector built for one AI model wouldn't work with another.
- There was **no standard** for how tools should describe themselves to an AI.

MCP solves this by defining a single protocol. Now:
- Any AI that supports MCP can use **any MCP server** without custom code.
- Developers build an MCP server once and it works with all compatible AI tools.
- The AI knows exactly what tools are available and how to call them.

---

## How It Works

1. **An MCP server is started** — it's a small program that "wraps" a capability (e.g., your file system, a database, a web search API).
2. **The server registers its tools** — it tells the AI: "Here are the things I can do," described in a structured format (name, description, required inputs).
3. **The AI receives a task** — e.g., "Summarize my notes from last week."
4. **The AI decides which tool to call** — it looks at available MCP servers, picks the right one (e.g., file-reader), and sends a request with the needed parameters.
5. **The MCP server executes the task** — reads the files, queries the DB, runs the search, etc.
6. **The server returns the result** — the AI receives the output and continues its response.

```
User ──► AI Model ──► MCP Client ──► MCP Server ──► External Resource
                                         (files, DB, API, browser...)
         ◄────────────────────────────────────────────────────────────
```

---

## Key Terms

| Term | What it means |
|------|---------------|
| **MCP** | Model Context Protocol — the open standard created by Anthropic |
| **MCP Server** | A program that exposes tools/resources to an AI via MCP |
| **MCP Client** | The AI-side component that connects to and calls MCP servers |
| **Tool** | A specific capability exposed by an MCP server (e.g., `read_file`, `search_web`) |
| **Resource** | A data source an MCP server can expose (e.g., a file, a database row) |
| **Transport** | How MCP messages travel — usually `stdio` (local) or `HTTP/SSE` (remote) |
| **stdio** | Standard input/output — a way for two programs on the same machine to talk to each other |

---

## In Depth

### Architecture / Structure

MCP follows a **client-server architecture** built on [[JSON-RPC 2.0]]:

- **Host**: The application running the AI (e.g., Claude Desktop, VS Code Copilot). It manages MCP client connections.
- **Client**: Lives inside the host. Maintains a 1-to-1 connection with one MCP server.
- **Server**: An external process that exposes capabilities. Can be local (running on your machine) or remote (running on a cloud server).

MCP servers can expose three types of primitives:
| Primitive | Description | Example |
|---|---|---|
| **Tools** | Functions the AI can call | `run_query(sql)`, `read_file(path)` |
| **Resources** | Data the AI can read | file contents, database rows |
| **Prompts** | Pre-written prompt templates | a code review template |

### Transports

| Transport | Use case |
|---|---|
| `stdio` | Local servers — the host spawns the server as a subprocess |
| `HTTP + SSE` | Remote servers — communicates over the network |

### How It's Used in Practice

- **Claude Desktop** ships with MCP support built in. You configure servers in a JSON config file and Claude can immediately use their tools.
- **VS Code / GitHub Copilot** supports MCP servers added to workspace settings.
- **Custom servers** are often written in Python or TypeScript using official SDKs.

Example MCP config in Claude Desktop (`claude_desktop_config.json`):
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/folder"]
    }
  }
}
```

### Common Gotchas / Misconceptions

- **MCP is not an AI model** — it's a protocol. The intelligence still comes from the LLM; MCP just gives the LLM hands to reach into external systems.
- **MCP servers don't run inside the AI** — they're separate processes. The AI sends a request; the server does the work and returns a result.
- **Not all AI tools support MCP** — it's open-source and growing, but adoption varies. Claude and GitHub Copilot support it; others may not yet.
- **Security matters** — an MCP server with file system access can read (or write) files on your machine. Only connect to trusted MCP servers.

---

## Quick Reference

```bash
# Install a common MCP server via npm
npx -y @modelcontextprotocol/server-filesystem /path/to/folder

# List available tools from an MCP server (using the MCP inspector)
npx @modelcontextprotocol/inspector
```

---

## Related Notes

- [[MOC - AI]] — parent index for all AI concepts
- [[JSON-RPC]] — the underlying message format MCP uses
- [[AI Agent]] — agents are the primary users of MCP tools
- [[Claude]] — the AI model created by Anthropic that pioneered MCP
- [[Setting Up an MCP Server]]
- 
## References

- [Model Context Protocol — Official Docs](https://modelcontextprotocol.io)
- [MCP GitHub Repository](https://github.com/modelcontextprotocol)
- [Anthropic MCP Announcement](https://www.anthropic.com/news/model-context-protocol)
