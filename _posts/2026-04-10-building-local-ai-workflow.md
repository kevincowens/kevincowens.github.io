---
title: "Building My Local AI Workflow"
date: 2026-04-10
permalink: /blog/building-local-ai-workflow/
---

# Building My Local AI Workflow

After months of experimenting, I finally have a local AI workflow that I actually use every day. Here's what it looks like and why I built it this way.

## The Problem

Cloud-based LLM APIs are convenient, but they have real limitations for a homelab:

- **Privacy** — I don't want operational notes, config snippets, or personal context sent to third-party servers.
- **Cost** — API calls add up fast when you're running automation pipelines continuously.
- **Latency** — Round-trips to external APIs slow down local automation loops.

The solution: run everything locally.

## The Stack

```
Ollama (inference) → n8n (orchestration) → local file system / Git
```

### Ollama

[Ollama](https://ollama.ai) makes running open-source LLMs dead simple. I'm currently running:

- **llama3** — general-purpose reasoning and writing
- **mistral** — fast summarization and quick tasks
- **nomic-embed-text** — embeddings for semantic search

All models run on a single machine with a consumer GPU. Performance is more than adequate for non-real-time workflows.

### n8n

n8n is the automation backbone. It connects:

- Webhook triggers (from GitHub, cron, or manual)
- Ollama API calls
- Local file reads/writes
- Git commits via shell nodes

The key advantage over something like LangChain is that n8n gives me a visual audit trail — I can see every workflow step, debug failures easily, and hand off workflows to non-developers.

### The File System as a Database

I deliberately avoid databases for this workflow. Everything is Markdown files in a Git repo. Reasons:

1. **Portable** — any tool can read/write Markdown
2. **Auditable** — every change is a commit
3. **LLM-native** — models read Markdown naturally, no schema translation needed

## A Simple Example: Daily Briefing

Every morning at 6 AM, an n8n workflow:

1. Reads my open GitHub issues and project notes
2. Sends them to Ollama with a system prompt: *"Summarize the most important items and suggest the highest-priority next action."*
3. Writes the output to `briefing/YYYY-MM-DD.md`
4. Commits and pushes

I open my terminal, read the briefing, and start work. No browser, no context switching.

## What's Next

I'm working on adding a retrieval layer — embedding all my notes and using semantic search to give the LLM relevant context automatically. More on that in a future post.
