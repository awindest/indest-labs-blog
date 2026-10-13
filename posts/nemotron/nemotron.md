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

<Image src="nemotron3.webp" title="Can't wait to start playing with this model!" alt="Picture of Nemotron 3 Ultra logo." />

Today's AI models (Large Language Models or LLMs) are getting better and better. Nvidia's Nemotron 3 Ultra LLM emerges as the latest proof point. Here we show you how you can gain access to this model without worrying about paying for it. Yes, you are limited in usage but this lowering of the cost of access seems to be the beginning of a trend. Remember when texting was priced by the text?

The instructions to setup your own Nemotron 3 instance follow.

## Nvidia

1. Create an Nvidia account and login.
1. Go to https://build.nvidia.com
1. You will be asked to create an Nvidia cloud account - pick a name and continue.
1. You will be asked to enter your phone number so that you will get a one-time verification password.
1. You should be ready to search for the Nemotron model. Find the model, and click on it; for example:
<Image src="nemotron3-search.webp" alt="Picture of Nemontron 3 Ultra model." />
1. Notice the description that this is a free API endpoint .
<Image src="nemotron3-model.webp" alt="Picture of Nemontron 3 Ultra logo." />
1. Generate a key by clicking the Generate API Key button: 
<Image src="nemotron3-generate.webp" alt="Picture of Generate API button." />
1. My key appeared in the python code. Here is my code snippet:
```bash
client = OpenAI(
  base_url = "https://integrate.api.nvidia.com/v1",
-->  api_key = "nvapi-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" <----
)
```

9. Save your API key somewhere safe otherwise you will have to generate a new one.

## Opencode
1. Install opencode terminal (or TUI), I used brew:
```bash
brew install anomalyco/tap/opencode
```
2. Launch opencode
```bash
opencode
```
3. Enter `/connect`
<Image src="nemotron3-connect.webp" alt="Picture of how to invoke the provider menu to connect." />
1. Then find Nvidia and select it.
<Image src="nemotron3-provider.webp" alt="Picture of Nemontron 3 Ultra logo." />
1. Find the Nemontron 3 Ultra entry and select it.
<Image src="nemotron3-selectultra.webp" alt="Picture of Nemontron 3 Ultra logo." />
1. Enter your key.
<Image src="nemotron3-apikey.webp" alt="Picture of an obfuscated API key." />
Boom! 💥 You have access to a coding model for free using `opencode`. Unfortunately there are limiters in place which will vary in duration over time, I'm sure but for curious use and medium strength tasking, this may suffice!

<Image src="nemotron3-meaningoflife.webp" alt="Picture of Nemontron 3 Ultra logo." />

Note: Hit Control-C to exit `opencode`.

And that's all there is to it. You now have an extremely powerful LLM at your fingertips. 