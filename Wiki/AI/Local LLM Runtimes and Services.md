---
title: "Local LLM Runtimes and Services"
date: 2026-05-17
tags:
  - wiki
  - ai/local
  - ai/infrastructure
  - ai/model
aliases:
  - "LLM Services Compared"
  - "Ollama vs vLLM"
  - "Local AI Tools"
---

# Local LLM Runtimes and Services

## Table of Contents
- [[#Simple Explanation]]
- [[#The Key Distinction — Models vs Software]]
- [[#Category 1 — Model Families (The Weights)]]
- [[#Category 2 — Inference Engines (The Core Runtime)]]
- [[#Category 3 — Local API Servers]]
- [[#Category 4 — Desktop Apps with a GUI]]
- [[#Category 5 — Cloud Inference Providers]]
- [[#Category 6 — Model Hubs]]
- [[#Side-by-Side Comparison]]
- [[#Which Should I Use?]]
- [[#Common Gotchas / Misconceptions]]
- [[#Related Notes]]

---

## Simple Explanation

"Ollama", "LLaMA", and "vLLM" sound similar but are completely different kinds of things. **LLaMA is a model** (the AI brain itself). **Ollama is an app** that downloads and runs models. **vLLM is a server** optimized for high-speed production use.

> **Analogy:** LLaMA is a song. Ollama is a music player app on your laptop. vLLM is a concert hall sound system built for playing to thousands of people simultaneously. They all involve the same "music" but serve completely different purposes.

---

## The Key Distinction — Models vs Software

Before comparing anything, understand this split:

| Type | What it is | You download... |
|---|---|---|
| **Model / Model Family** | The actual AI weights — the "brain" | Gigabytes of `.gguf` or `.safetensors` files |
| **Runtime / Server** | Software that loads and runs a model | A program or service |

Most confusion comes from mixing these two. LLaMA is always a model. Everything else on this page is software that can *run* LLaMA (and other models).

---

## Category 1 — Model Families (The Weights)

These are the actual AI models — not apps, not servers. They're files you download that contain the trained neural network parameters.

| Model Family | Made by | Notable versions | Strengths |
|---|---|---|---|
| **LLaMA** | Meta | LLaMA 2, LLaMA 3, LLaMA 3.1, LLaMA 3.3 | General purpose; most widely supported locally |
| **Mistral** | Mistral AI | Mistral 7B, Mixtral 8x7B, Mistral Small | Efficient; strong for size |
| **Gemma** | Google | Gemma 2B, Gemma 7B, Gemma 2 | Lightweight; good on consumer hardware |
| **Phi** | Microsoft | Phi-2, Phi-3, Phi-4 | Tiny but surprisingly capable |
| **Qwen** | Alibaba | Qwen2, Qwen2.5, QwQ | Strong multilingual and coding |
| **DeepSeek** | DeepSeek | DeepSeek R1, DeepSeek V3 | Strong reasoning; open weights |
| **Command R** | Cohere | Command R, Command R+ | Optimized for RAG and tool use |

**Model formats:**
- `.gguf` — compressed format used by llama.cpp-based tools (Ollama, LM Studio)
- `.safetensors` — standard format used by Hugging Face / Python tools (vLLM, Transformers)

---

## Category 2 — Inference Engines (The Core Runtime)

These are the low-level libraries that actually do the math of running a model. Most higher-level tools (Ollama, LM Studio) use one of these under the hood.

| Engine | Language | Best for | Used by |
|---|---|---|---|
| **llama.cpp** | C++ | CPU + consumer GPU; GGUF format | Ollama, LM Studio, koboldcpp |
| **vLLM** | Python/CUDA | High-throughput GPU serving; production | Standalone server |
| **ExLlamaV2** | Python/CUDA | Fast quantized GPU inference | text-generation-webui |
| **HF Transformers** | Python | Research; full model flexibility | Direct Python use, vLLM |
| **MLX** | Swift/Python | Apple Silicon (M1/M2/M3 Mac) | Ollama on Mac, MLX-LM |

---

## Category 3 — Local API Servers

These run on your machine and expose an API endpoint (usually OpenAI-compatible at `http://localhost:PORT`) so any app that can talk to OpenAI can talk to your local model instead.

### Ollama
- **Best for:** Easiest local setup; developer use; personal projects
- **Under the hood:** llama.cpp
- **API:** OpenAI-compatible (`http://localhost:11434`)
- **Model library:** `ollama pull llama3` — downloads from Ollama's registry
- **Platforms:** Windows, macOS, Linux
- **Strengths:** One command to download + run any model; zero config; great DX
- **Limitations:** Not optimized for high concurrency; single-user focused

```bash
# Install and run a model in two commands
ollama pull llama3.2
ollama run llama3.2
```

---

### vLLM
- **Best for:** Production servers; high-throughput; multiple concurrent users
- **Under the hood:** Custom CUDA kernels + PagedAttention algorithm
- **API:** OpenAI-compatible (`http://localhost:8000`)
- **Platforms:** Linux + NVIDIA GPU (primary); limited Windows support
- **Strengths:** Highest throughput of any open inference server; handles many simultaneous requests efficiently; supports tensor parallelism across multiple GPUs
- **Limitations:** Requires NVIDIA GPU; Linux preferred; more complex setup than Ollama

```bash
pip install vllm
python -m vllm.entrypoints.openai.api_server --model meta-llama/Llama-3.2-8B
```

---

### LM Studio (server mode)
- **Best for:** Easy local API without using the terminal
- **API:** OpenAI-compatible (`http://localhost:1234`)
- **Strengths:** GUI to browse/download models, then enable local server with one toggle
- See [[#Category 4 — Desktop Apps with a GUI]] — LM Studio is both a GUI app and a server

---

### Text Generation WebUI (oobabooga)
- **Best for:** Power users who want maximum control over generation parameters
- **API:** REST + WebSocket endpoints
- **Platforms:** Windows, Linux, macOS
- **Strengths:** Supports many backends (llama.cpp, ExLlamaV2, Transformers); character/persona UI; lots of extensions
- **Limitations:** Complex setup; UI can feel dated

---

## Category 4 — Desktop Apps with a GUI

For people who want to chat with local models without using a terminal or writing code.

| App | Platform | Under the hood | Best for |
|---|---|---|---|
| **LM Studio** | Windows, macOS, Linux | llama.cpp | Browsing, downloading, chatting with local models; also has a local server mode |
| **Jan** | Windows, macOS, Linux | llama.cpp / Nitro | Clean UI; also runs a local OpenAI-compatible server |
| **GPT4All** | Windows, macOS, Linux | llama.cpp | Simple desktop chatbot; beginner-friendly |
| **AnythingLLM** | Windows, macOS, Linux | Multiple | Full local RAG + chat app with document ingestion |

---

## Category 5 — Cloud Inference Providers

You don't run these locally — they host open models (LLaMA, Mistral, etc.) on their own hardware and give you an API. Useful when you want open models but don't have a GPU.

| Provider | Strengths | Notes |
|---|---|---|
| **Groq** | Extremely fast (custom LPU hardware); free tier | Best for speed; limited model selection |
| **Together AI** | Wide model selection; fine-tuning support | Good balance of price + variety |
| **Fireworks AI** | Fast; production-grade; function calling support | Good for apps |
| **Replicate** | Easy deployment of any Hugging Face model | Good for experimentation |
| **OpenRouter** | Routes to cheapest/best provider automatically | Meta-layer over many providers |

---

## Category 6 — Model Hubs

Places to browse, download, and share model weights.

| Hub | Notes |
|---|---|
| **Hugging Face** | The primary hub; hosts most open models in `.safetensors` format |
| **Ollama Library** | `ollama.com/library` — models pre-packaged for Ollama in `.gguf` format |
| **Civitai** | Focused on image generation models (Stable Diffusion, etc.) |

---

## Side-by-Side Comparison

| | LLaMA | Ollama | vLLM | LM Studio | Groq |
|---|---|---|---|---|---|
| **Type** | Model | Local server | Local server | Desktop app | Cloud API |
| **Runs where** | Your machine (via a runtime) | Your machine | Your machine | Your machine | Groq's servers |
| **GPU required** | No (slow without) | No (uses CPU) | Yes (NVIDIA) | No | N/A |
| **Setup difficulty** | N/A | Very easy | Medium-hard | Very easy | Easy (API key) |
| **Best for** | — | Dev / personal use | Production serving | Non-technical use | Speed without GPU |
| **Concurrent users** | — | Low (1-2) | High (100s) | Low (1-2) | High |
| **Cost** | Free weights | Free | Free | Free | Free tier + paid |

---

## Which Should I Use?

| Situation | Recommendation |
|---|---|
| Just want to try local AI | **LM Studio** or **Ollama** |
| Building an app that needs a local model API | **Ollama** |
| Running a model for multiple users / production | **vLLM** (Linux + NVIDIA) |
| On a Mac with Apple Silicon | **Ollama** (uses MLX under the hood on Mac) |
| Want fast API without owning a GPU | **Groq** |
| Need a wide selection of models via API | **Together AI** or **OpenRouter** |

---

## Common Gotchas / Misconceptions

- **"LLaMA is a program I install"** — No. LLaMA is model weights (files). You need a runtime like Ollama or vLLM to actually run it.
- **"Ollama and vLLM are competitors"** — They target different audiences. Ollama is for local dev; vLLM is for production servers. You might use both.
- **"Bigger model = better"** — Not always. A well-tuned 8B model often beats a poorly-tuned 70B. Size matters less than training quality and quantization.
- **"You need a GPU"** — For fast inference, yes. llama.cpp can run on CPU-only, just slowly. Small models (3B–7B) are usable on modern CPUs.
- **"GGUF and safetensors files are the same"** — They're different formats. GGUF is for llama.cpp-based tools (Ollama, LM Studio). Safetensors is for Hugging Face / Python-based tools (vLLM, Transformers). Same model, different packaging.

---

## Related Notes
- [[LLM Wrappers Agents and Frameworks]] — how software layers on top of these runtimes
- [[MCP Server]] — how to give a locally-running model access to tools
- [[MOC - AI]] — parent index
