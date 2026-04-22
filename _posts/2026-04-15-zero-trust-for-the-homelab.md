---
title: "Zero-Trust for the Homelab"
date: 2026-04-15
permalink: /blog/zero-trust-for-the-homelab/
---

# Zero-Trust for the Homelab

"Zero-Trust" gets thrown around a lot in enterprise security. Most homelab guides ignore it entirely — flat networks, open internal ports, everything trusted by default. I wanted to do better.

## What Zero-Trust Actually Means

The core principle: **never trust, always verify.** No device, user, or service is automatically trusted just because it's on the internal network.

Practically, this means:

- Every service requires explicit authentication, even on LAN
- Lateral movement between services is blocked by default
- Network segments are isolated
- Access is logged and auditable

## My Threat Model

Before implementing anything, I wrote down what I'm actually defending against:

1. **Compromised IoT devices** — cameras, smart plugs, etc. spreading laterally
2. **A vulnerable service** — an exposed port on a container gets exploited
3. **Misconfiguration** — I make a mistake opening something I shouldn't

I'm not defending against nation-state attackers. I'm defending against the realistic risks of running a complex home network with lots of moving parts.

## The Implementation

### Network Segmentation with VLANs

I use VLANs to separate:

| VLAN | Purpose |
|------|---------|
| Management | Servers, NAS, proxmox nodes |
| Services | Containers, self-hosted apps |
| IoT | Smart home devices |
| Trusted | Personal devices |
| Guest | Isolated internet access |

Inter-VLAN routing is blocked by default at the firewall. Explicit allow rules are added only where needed.

### Tailscale for Remote Access

Instead of opening ports to the internet, I use [Tailscale](https://tailscale.com). Every device I own joins a private mesh network. Access to internal services goes through Tailscale — no port forwarding, no exposed SSH.

This also solves the "traveling laptop" problem. My laptop automatically connects to homelab services over an encrypted tunnel regardless of where I am.

### Container Isolation

Every service runs in its own container with:

- A dedicated network namespace
- No access to the host network
- Explicit port mappings for only required ports
- Read-only bind mounts where possible

I use Podman over Docker for the additional rootless isolation.

### Secrets Management

No secrets in environment variables or config files checked into Git. I use a local Vault instance to issue short-lived credentials. Services authenticate to Vault via AppRole and retrieve secrets at startup.

## Lessons Learned

**Start with the threat model.** I wasted weeks building the wrong defenses before I wrote down what I was actually worried about.

**Tailscale is the single highest-value change.** Closing all external ports and routing everything through a zero-trust mesh network eliminated an entire attack surface.

**Logs are only useful if you read them.** I built a weekly log review into my automation workflow using Ollama to summarize anything anomalous.

## What's Next

I'm working on adding automated compliance checks — scripts that verify firewall rules, container configs, and secret rotation status, then report deviations. Think of it as a continuous audit loop.
