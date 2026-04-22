---
title: "Deploying n8n with Secure Boundaries"
date: 2026-04-20
permalink: /blog/deploying-n8n-with-secure-boundaries/
---

# Deploying n8n with Secure Boundaries

n8n is incredibly powerful — a self-hosted automation platform that connects anything to anything. It's also the kind of tool that can become a serious security liability if you deploy it carelessly. Here's how I deploy it with proper security boundaries.

## Why n8n is High-Risk

n8n can:

- Execute arbitrary shell commands via the Execute Command node
- Make HTTP requests to any URL
- Read and write files on the host system
- Access environment variables

If an attacker gets access to your n8n instance, they effectively have access to everything n8n has credentials for — which, in a homelab AI workflow, can be quite a lot.

## The Deployment Setup

### Running in a Container with Limited Permissions

```yaml
# docker-compose excerpt
services:
  n8n:
    image: n8nio/n8n:latest
    user: "1000:1000"
    read_only: true
    tmpfs:
      - /tmp
    volumes:
      - n8n_data:/home/node/.n8n:rw
      - ./workflows:/workflows:ro
    networks:
      - n8n_internal
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_SECURE_COOKIE=true
      - EXECUTIONS_DATA_PRUNE=true
```

Key points:
- Runs as a non-root user (uid 1000)
- Read-only filesystem with tmpfs for temporary files
- Workflow files mounted read-only
- Isolated to its own internal network

### No Direct Internet Exposure

n8n never has a public IP. Access is only via Tailscale. The only outbound connections allowed from the n8n container are to:

- The Ollama API (internal)
- GitHub API (for workflow triggers)
- Specific webhook destinations I've explicitly approved

I enforce this with container network policies on the firewall.

### Credential Isolation

Each external service gets its own limited API token — not a personal access token, not admin credentials. When a token needs rotation, I update it in Vault and restart the affected workflow.

n8n's built-in credential store is fine for low-sensitivity integrations (like a read-only GitHub token). For anything that touches production systems or has write access, credentials come from Vault at runtime.

### Execution Logging

Every workflow execution is logged. I export execution logs to a local Loki instance and run a nightly Ollama-powered summary:

- How many executions ran?
- Were there any failures?
- Did any workflow make unusual external connections?

This isn't foolproof, but it means I'd notice if a workflow started behaving unexpectedly.

## Workflow Design Principles

Security isn't just infrastructure — workflow design matters too.

**Principle of least privilege in nodes.** If a workflow only needs to read a file, it uses a read-only credential. I don't reuse credentials across workflows.

**Validate inputs at webhook triggers.** Any webhook-triggered workflow validates the payload before processing. Unexpected fields are dropped, not passed downstream.

**Error handling everywhere.** Every workflow has an error branch that logs the failure and sends me a notification. Silent failures are a security risk — they mask compromised workflows.

## What I'd Do Differently

If I were starting from scratch, I'd set up Vault integration from day one instead of retrofitting it later. Migrating credentials out of n8n's built-in store is tedious work.

I'd also deploy n8n behind an identity-aware proxy (like Authentik) from the beginning, rather than relying on basic auth.

## Summary

n8n is worth the complexity — but treat it like a privileged service from day one. The defaults are convenient, not secure.
