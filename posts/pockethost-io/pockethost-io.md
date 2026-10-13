---
title: A Story about PocketHost.io
description: An interesting story about Ben Allfree, the person behind pockethost.io, a free pocketbase (as of today) cloud service.
slug: pockethost-io
published: '2023-12-10'
category: svelte
---
<script lang="ts"> 
import Embed from '#lib/components/embed.svelte' 
import Image from '#lib/components/image.svelte' 
import YouTube from '#lib/components/youtube.svelte' 
</script>

<Image src="pockethost.avif" alt="PocketHost.io logo from web site"/>

### Update

Well, they have begun charging, but not too much, for an instance of pocketbase. **sigh**

### Introduction

Found on reddit:

```
Hello, creator of PocketHost here. Someone from the
Discord server shared this link.

PocketHost was created to support my own projects, and
other people found it useful. As of this writing,
there are over 3,000 users and ~~4,000~~ 5,000 instances
of PocketBase running.

It's free because I pay for it and don't
charge anyone :)

Maybe some day there will be a paid tier, but for now,
please enjoy PocketHost without restrictions of any kind.

The paid tier that feels best to me is something like
$20/mo for unlimited instances, bandwidth, and storage.
Because of how PocketHost is used, who uses it, and my
personal values, a flat access fee makes more sense than
metering out every byte and CPU cycle. I don't think
PocketHost is a utility computing hosting provider.
It's a specialized hosting provider to make an awesome
user experience for devs and makers looking to maintain
sovereignty while getting up and running quickly.


```

### Reaction

Wow! What is going on? I dug a little deeper an found this
person is the real deal. He may make money later but right now
the hot PocketBase database is free... and easy to use.

You can explore more at
[https://pockethost.io](https://pockethost.io)

So how does it work? First you create an account at
PocketHost.io and then create a pocketbase collection. Mine
is called `posts`. Then you read the docs and figure out that
you access the database via a REST API that is simple to use.

### Deployment

Using [`vercel`](https://vercel.com) makes deployment so much easier. After testing
your code you commit it to github and if you have linked your
github project to `vercel` it is automatically deployed and available
world-wide on either `<your domain>.vercel.app` or your
custom domain. Pretty cool.
