---
title: Nemotron 3 Ultra for Free!
description: How to setup a great model with opencode.
slug: nemotron
published: '2026-10-2'
category: AI
---

<script lang="ts"> 
import Embed from '#lib/components/embed.svelte' 
import Image from '#lib/components/image.svelte' 
import YouTube from '#lib/components/youtube.svelte' 
</script>

<Image src="nemotron3.webp" alt="Picture of Nemontron 3 Ultra logo." />

## Nvidia

1. Create an Nvidia account and login

1. Go to https://build.nvidia.com

1. You will be asked to create an Nvidia cloud account - pick a name and continue

1. You will be asked to enter your phone number so that you will get a one-time verification password

1. You should be ready to search for the Nemotron model. Find the model, and click on it; for example:

<Image src="nemotron3-search.webp" alt="Picture of Nemontron 3 Ultra model." />

1. Notice the free API endpoint.

<Image src="nemotron3-model.webp" alt="Picture of Nemontron 3 Ultra logo." />

1. Generate a key by clicking the Generate API Key button: (mine appeared in the python code)

<Image src="nemotron3-generate.webp" alt="Picture of Generate API button." />

1. Here is a code snippet:

```bash
client = OpenAI(
  base_url = "https://integrate.api.nvidia.com/v1",
-->  api_key = "nvapi-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" <----
)
```

1. Save your API key somewhere safe otherwise you will have to generate a new one.


## Opencode

9 - Install opencode terminal (or TUI), I used brew:

```bash
brew install anomalyco/tap/opencode
```

1. Launch opencode

```bash
opencode
```

1. Enter `/connect`


1. Then find Nvidia and select it

<Image src="nemotron3-connect.webp" alt="Picture of how to invoke the provider menu to connect." />
1. Enter your key

<Image src="nemotron3-apikey.webp" alt="Picture of an obfuscated API key." />

<Image src="nemotron3-provider.webp" alt="Picture of Nemontron 3 Ultra logo." />

<Image src="nemotron3-selectultra.webp" alt="Picture of Nemontron 3 Ultra logo." />

And now you have access to a coding model for free. Unfortunately there are limiters in place which will vary over time, I'm sure.

<Image src="nemotron3-meaningoflife.webp" alt="Picture of Nemontron 3 Ultra logo." />

Note: Hit Control-C to get out of `opencode`.
