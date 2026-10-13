<script lang="ts">
	import { page } from '$app/state'
	import { dev } from '$app/env'
	import { imagesLocal, imagesUrl } from '#lib/site/config.js'

	type Props = {
		alt: string
		src: string
		slug: string
		title?: string
	}

	let { src, title, alt }: Props = $props()

	// const images = dev ? imagesLocal : imagesUrl
	const images = dev ? imagesLocal : imagesUrl
	const slug = page.params.slug
</script>

<img src="{images}/{slug}/images/{src}" {title} {alt} loading="lazy" />
