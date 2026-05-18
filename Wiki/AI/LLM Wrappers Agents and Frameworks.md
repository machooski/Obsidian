---
title: "LLM Wrappers, Agents, Frameworks, and Harnesses"
date: 2026-05-17
tags:
  - wiki
  - ai/terminology
  - ai/agent
  - ai/framework
aliases:
  - "LLM Terminology"
  - "AI Layers"
  - "LLM Wrappers vs Agents"
---

# LLM Wrappers, Agents, Frameworks, and Harnesses

## Table of Contents
- [[#Simple Explanation]]
- [[#The Spectrum — From Simple to Complex]]
- [[#Term Breakdown]]
  - [[#SDK / LLM Wrapper]]
  - [[#LLM Framework]]
  - [[#Agent]]
  - [[#Agent Framework]]
  - [[#Orchestrator]]
  - [[#Harness / Eval Framework]]
- [[#Visual Map]]
- [[#Real-World Examples]]
- [[#Common Gotchas / Misconceptions]]
- [[#Related Notes]]

---

## Simple Explanation

All these terms describe **how much responsibility the software takes on** when working with an LLM.

At one end: a wrapper that just makes the API call for you. At the other: a full multi-agent system where several AIs collaborate, use tools, and check each other's work — all automatically.

> **Analogy:** Think of it like a kitchen.
> - **Wrapper** = a phone that lets you call the chef to order food
> - **Framework** = a recipe book with prep steps, timers, and a shopping list
> - **Agent** = a sous chef who follows the recipe, adapts when ingredients are missing, and asks for help
> - **Orchestrator** = the head chef directing multiple sous chefs
> - **Harness** = a food critic scoring each dish against a rubric

---

## The Spectrum — From Simple to Complex

```
Raw API call
    ↓
SDK / LLM Wrapper       (abstracts the HTTP call)
    ↓
LLM Framework           (adds chains, memory, retrieval)
    ↓
Agent                   (LLM + tools + reasoning loop)
    ↓
Agent Framework         (build/run multiple agents)
    ↓
Orchestrator            (routes tasks between agents/models)
```

Harnesses / Eval frameworks sit **outside** this chain — they test and score the above.

---

## Term Breakdown

### SDK / LLM Wrapper

**What it is:** A thin library that simplifies making API calls to an LLM. Handles authentication, request formatting, retries, and response parsing — nothing more.

**What it does NOT do:** Memory, tool use, chaining, decision-making.

| Examples | Notes |
|---|---|
| `openai` (Python/JS) | Official OpenAI SDK |
| `anthropic` (Python/JS) | Official Anthropic SDK |
| `@google/generative-ai` | Google Gemini SDK |
| `litellm` | Unified wrapper for 100+ providers — same code works with any model |

---

### LLM Framework

**What it is:** A higher-level library that adds structure for building LLM-powered applications — prompt templates, memory, document retrieval (RAG), chains of calls, and tool integration.

**Think of it as:** Plumbing and scaffolding for LLM apps. You still control the flow; the framework just makes the pipes easier to build.

| Examples | Notes |
|---|---|
| **LangChain** | Most popular; chains, agents, RAG, 100s of integrations |
| **LlamaIndex** | Focused on RAG and knowledge retrieval over documents |
| **Haystack** | Pipeline-based; strong for search and document QA |
| **DSPy** | Optimizes prompts automatically through programming |

---

### Agent

**What it is:** An LLM placed in a **think → act → observe loop**. It receives a goal, decides which tool to call, receives the result, and repeats until the task is done.

**Key property:** The LLM makes decisions at runtime — it's not following a fixed script.

The loop looks like:
1. **Think** — LLM reasons about what to do next
2. **Act** — calls a tool (web search, code execution, file read, MCP server, etc.)
3. **Observe** — receives the tool's output
4. **Repeat** until goal is complete or it decides it's done

**Note:** An agent is a *behavior pattern*, not a specific library. You can have an agent built with LangChain, with raw API calls, or with an MCP-connected Claude.

---

### Agent Framework

**What it is:** A library specifically designed for building, running, and coordinating agents — especially **multiple agents** that collaborate or check each other.

| Examples | Notes |
|---|---|
| **LangGraph** | Graph-based agent orchestration; fine-grained control over agent flow |
| **CrewAI** | Role-based multi-agent teams (Researcher, Writer, Reviewer, etc.) |
| **AutoGen** | Microsoft's framework; agents that converse with each other |
| **OpenAI Agents SDK** | OpenAI's official framework for building agentic workflows |
| **Pydantic AI** | Type-safe agent framework; uses Pydantic for structured outputs |

---

### Orchestrator

**What it is:** A component (or pattern) that **routes tasks** between multiple agents, models, or tools. Decides which specialist to call for which sub-task.

**Difference from Agent Framework:** An orchestrator can coordinate agents without itself being an LLM — it can be pure logic, a router model, or a human-in-the-loop.

**Example flow:**
```
User: "Write a research report on MCP servers"
  → Orchestrator sends "search for sources" to Research Agent
  → Orchestrator sends sources to Writer Agent
  → Orchestrator sends draft to Reviewer Agent
  → Returns final output to user
```

---

### Harness / Eval Framework

**What it is:** Infrastructure for **testing and scoring** LLM outputs. You define test cases (input + expected behavior), run them through your LLM app, and get metrics back.

**Why it matters:** LLMs are non-deterministic — the same prompt can return different answers. Evals catch regressions when you change a prompt or swap a model.

| Examples | Notes |
|---|---|
| **promptfoo** | Open-source; YAML test cases, many model providers |
| **LangSmith** | LangChain's eval + tracing platform |
| **Braintrust** | Eval + logging platform with a UI |
| **OpenAI Evals** | OpenAI's own eval framework |
| **RAGAS** | Specialized for evaluating RAG pipelines |

---

## Visual Map

```
┌─────────────────────────────────────────────────────────────┐
│                      YOUR APPLICATION                        │
│                                                             │
│  ┌───────────────┐    ┌───────────────┐                     │
│  │  Orchestrator │───►│  Agent        │──► Tools/MCP        │
│  │               │    │  (think/act   │    Servers          │
│  │               │◄───│   loop)       │◄── Results          │
│  └───────────────┘    └───────────────┘                     │
│         │                    │                              │
│         ▼                    ▼                              │
│  ┌─────────────────────────────────┐                        │
│  │       LLM Framework             │                        │
│  │  (memory, RAG, prompt templates)│                        │
│  └─────────────────────────────────┘                        │
│                    │                                        │
│                    ▼                                        │
│  ┌─────────────────────────────────┐                        │
│  │       SDK / LLM Wrapper         │                        │
│  │  (API call, auth, retries)      │                        │
│  └─────────────────────────────────┘                        │
│                    │                                        │
│                    ▼                                        │
│              LLM API (Claude, GPT-4, etc.)                  │
│                                                             │
│  ┌─────────────────────────────────┐  ← runs separately     │
│  │       Harness / Eval            │                        │
│  │  (tests, scores, metrics)       │                        │
│  └─────────────────────────────────┘                        │
└─────────────────────────────────────────────────────────────┘
```

---

## Real-World Examples

| What you're doing | What you're using |
|---|---|
| Calling Claude from Python | SDK (anthropic library) |
| Building a chatbot with memory and document Q&A | LLM Framework (LangChain + LlamaIndex) |
| AI that searches the web and writes a report autonomously | Agent |
| AI research team: researcher + writer + editor AIs | Agent Framework (CrewAI) |
| Routing user questions to specialist sub-agents | Orchestrator |
| Verifying your prompt changes didn't break anything | Harness (promptfoo) |
| Connecting Claude to your files/databases | MCP Server |

---

## Common Gotchas / Misconceptions

- **"LangChain is for agents"** — LangChain is a framework that *can* build agents, but it's primarily a general LLM framework. LangGraph (built on LangChain) is the agent-specific part.
- **"An agent is a product"** — An agent is a pattern/behavior. You build one; you don't download one.
- **"Harnesses test prompts"** — They test your *entire pipeline* (prompt + model + retrieval + output parsing), not just the prompt text.
- **"More layers = better"** — For simple tasks, a plain SDK call is better than wrapping it in a full framework. Don't over-engineer.

---

## Related Notes
- [[MCP Server]] — the protocol that gives agents their tools
- [[Local LLM Runtimes and Services]] — where the actual LLM runs
- [[MOC - AI]] — parent index
