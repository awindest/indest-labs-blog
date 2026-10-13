---
title: OpenClaw and NemoClaw
description: How to install NemoClaw on a Mac and get OpenClaw
slug: nemoclaw
published: '2026-11-15'
category: AI
---

<script lang="ts"> 
import Embed from '#lib/components/embed.svelte' 
import Image from '#lib/components/image.svelte' 
import YouTube from '#lib/components/youtube.svelte' 
</script>

OpenClaw is a 

NemoClaw tames OpenClaw


You cannot natively install NVIDIA NemoClaw directly on macOS to point at LM Studio, because NVIDIA NemoClaw is a Linux-only open-source stack designed to run sandboxed OpenClaw agents inside an NVIDIA OpenShell container runtime. Yes, that is a mouthful and perhaps I should draw a picture. But, you can successfully install NemoClaw/OpenClaw on a Mac by running the NemoClaw stack inside Docker Desktop for Mac and pointing its inference provider to the OpenAI-compatible API server hosted locally by LM Studio.

Here are the steps to installing NemoClaw on an Apple Silicon Mac using a local LLM served by LM Studio.

Step 1: Start the Local LLM in LM Studio

1. Open LM Studio on your Mac.

1. Search for and download your preferred model (such as a GGUF/MLX format model).

1. Go to the Chat or Local Server tab, select your loaded model, and ensure hardware acceleration is set to MLX (Apple Silicon GPU) for best performance.

1. Start the local inference server (usually hosted at http://localhost:1234 by default with an OpenAI-compatible API format)

Step 2: Install NemoClaw on macOS

Open your Mac's Terminal application. (I use iTerm2)

1. Run the official installation script, bypassing the default Express/NVIDIA cloud model provider prompt to use a custom local setup (you did preview the script before installing, right?):

```bash
NEMOCLAW_NO_EXPRESS=1 curl -fsSL https://nvidia.com/nemoclaw.sh | bash
```

1. This downloads Node.js, installs the NVIDIA OpenShell runtime, builds the CLI, and sets up your secure local sandbox.

Step 3: Configure NemoClaw to Use LM Studio. 

1. During the interactive onboarding wizard (or via your local environment configs/policies), decline cloud providers and select a custom/OpenAI-compatible local provider endpoint.

1. Set your custom base URL to point to LM Studio's local server:Base URL: 
http://localhost:1234/v1

1. Complete the rest of the onboarding steps (naming your agent, applying policy tiers, and setting optional network/search presets)