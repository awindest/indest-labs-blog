---
title: OpenClaw and NemoClaw
description: How to install NemoClaw on a Mac and get OpenClaw
slug: nemoclaw
published: '2026-11-15'
category: AI
draft: false
---

<script lang="ts"> 
import Embed from '#lib/components/embed.svelte' 
import Image from '#lib/components/image.svelte' 
import YouTube from '#lib/components/youtube.svelte' 
</script>

<Image src="nemoclaw.webp" alt="Picture of OpenClaw and NemoClaw." />

## Overview

`OpenClaw` is a 


`NemoClaw` tames OpenClaw

`NemoClaw` is an open source reference stack for running certain AI agents more safely inside `OpenShell` sandboxes. With `NemoClaw`, you get guided onboarding, managed inference, network policy, managed integrations, OpenShell-backed state persistence, and lifecycle operations through the NemoClaw command-line interface (CLI) and its agent-specific aliases.

You cannot natively install NVIDIA NemoClaw directly on macOS to point at LM Studio, because NVIDIA NemoClaw is a Linux-only open-source stack designed to run sandboxed OpenClaw agents inside an NVIDIA OpenShell container runtime. Yes, that is a mouthful and perhaps I should draw a picture. But, you can successfully install NemoClaw/OpenClaw on a Mac by running the NemoClaw stack inside Docker Desktop for Mac and pointing its inference provider to the OpenAI-compatible API server hosted locally by LM Studio.

Here's how to do it.

## Installation

### Overview

Here are the steps to installing NemoClaw on an Apple Silicon Mac using a local LLM served by LM Studio.

Prior to installing, you need to have Docker Desktop running, load a model into LM Studio and start the LM Studio server. 

1. Start Docker
1. Start LM Studio server
1. Run the NemoClaw installer script

### LM Studio

Step 1: Start the Local LLM in LM Studio

1. Open LM Studio on your Mac.
1. Search for and download your preferred model. I downloaded the qwen/qwen3.5 9b MLX model.
1. Leverage the MLX format for an M1 Macbook Pro is the preferred way to go as acceleration is set for the Apple Silicon GPU and native use of Apple's unified memory so you get the best performance. The GGUF format will work but it is for cross-platform use.
1. Start the local inference server (usually hosted at http://localhost:1234 by default with an OpenAI-compatible API format)

### NemoClaw


   ███╗   ██╗███████╗███╗   ███╗ ██████╗  ██████╗██╗      █████╗ ██╗    ██╗
   ████╗  ██║██╔════╝████╗ ████║██╔═══██╗██╔════╝██║     ██╔══██╗██║    ██║
   ██╔██╗ ██║█████╗  ██╔████╔██║██║   ██║██║     ██║     ███████║██║ █╗ ██║
   ██║╚██╗██║██╔══╝  ██║╚██╔╝██║██║   ██║██║     ██║     ██╔══██║██║███╗██║
   ██║ ╚████║███████╗██║ ╚═╝ ██║╚██████╔╝╚██████╗███████╗██║  ██║╚███╔███╔╝
   ╚═╝  ╚═══╝╚══════╝╚═╝     ╚═╝ ╚═════╝  ╚═════╝╚══════╝╚═╝  ╚═╝ ╚══╝╚══╝


Step 2: Install NemoClaw on macOS

Open your Mac's Terminal application. (I use iTerm2)

1. Run the official installation script, bypassing the default Express/NVIDIA cloud model provider prompt to use a custom local setup (you did preview the script before installing, right?):
```bash
curl -fsSL https://nvidia.com/nemoclaw.sh | bash
```
1. This downloads `Node.js`, installs the NVIDIA OpenShell runtime, builds the CLI, and sets up your secure local sandbox.

  ```bash
  Type 'yes' to accept the NemoClaw license and third-party software notice and continue [no]: yes
  ```

Selected:
    `1) OpenClaw — Gateway-based AI agent with plugin ecosystem (openclaw.ai)`

Output:

`  ✓ openshell CLI: openshell 0.0.116
  ✓ Port 8080 available (OpenShell gateway)
  ✓ Apple GPU detected: Apple M1 Pro (16 cores), 16384 MB unified memory
  ⓘ Local NIM unavailable — requires NVIDIA GPU
  ⓘ Sandbox GPU: disabled (no NVIDIA GPU detected)`

Step 3: Configure NemoClaw to Use LM Studio. 

1. During the interactive onboarding wizard (or via your local environment configs/policies), decline cloud providers and select a custom/OpenAI-compatible local provider endpoint.

  Select your inference provider:
    1) NVIDIA Endpoints
    2) OpenRouter
    3) OpenAI
    4) Other OpenAI-compatible endpoint
    5) Anthropic
    6) Other Anthropic-compatible endpoint
    7) Google Gemini
    8) Install Ollama (macOS)
    9) Model Router (experimental)
    10) Local llama.cpp
1. Set your custom base URL to point to LM Studio's local server:
`Base URL: http://localhost:1234/v1`
1. You don't need a key
1. Other OpenAI-compatible endpoint model []: qwen/qwen3.5-9b
1.   Sandbox name (1-19 characters, lowercase, starts with a letter, letters/numbers/single internal hyphens only, ends with letter/number) [my-assistant]:`capt-nemo`
1. Output:
```bash
  ──────────────────────────────────────────────────
  Review configuration
  ──────────────────────────────────────────────────
  Provider:      compatible-endpoint
  Model:         qwen/qwen3.5-9b
  API key:       configured for OpenShell gateway registration
  Web search:    disabled
  Managed tools: none
  Messaging:     none
  Sandbox name:  capt-nemo
  Note:          Sandbox build typically takes 5–15 minutes on this host.
  ──────────────────────────────────────────────────
  Web search and messaging channels will be prompted next.
  Choose an action:
    1) Apply configuration
    2) Edit inference provider or model
    3) Edit sandbox name
    4) Exit onboarding
```
Selected 1) Apply configuration

1.  [4/8] Setting up inference provider
  ──────────────────────────────────────────────────
  ✓ Reusing selected Chat Completions validation: compatible-endpoint / qwen/qwen3.5-9b
  ✓ Inference route set: compatible-endpoint / qwen/qwen3.5-9b

  Enable web search for your agent?
    [1] No web search (default)
    [2] Brave Search
    [3] Tavily Search
  Choose [1-3]:

  Selected 1


1.   Available messaging channels:
    [1] ○ telegram — Telegram bot messaging
    [2] ○ discord — Discord bot messaging
    [3] ○ wechat — WeChat (personal) bot messaging
    [4] ○ slack — Slack bot messaging
    [5] ○ whatsapp — WhatsApp Web messaging (QR pairing)
    [6] ○ teams — Microsoft Teams bot messaging (experimental)
    [7] ○ googlechat — Google Chat (Chat API) bot messaging (experimental)

    Selected none.
1.   Resource profiles:
    1) creator (cpu=50%, ram=50%)
    2) gamer (cpu=25%, ram=25%)
    3) game-developer (cpu=60%, ram=60%)
    4) developer (cpu=75%, ram=75%)
    5) custom (enter values manually)
    6) No profile (OpenShell defaults)
    Selected 6
1.
1. Complete the rest of the onboarding steps (naming your agent, applying policy tiers, and setting optional network/search presets)

