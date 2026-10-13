<script lang="ts">
	import Header from '#lib/ui/header/header.svelte'
	import Footer from '#lib/ui/footer.svelte'
	import LiteYouTubeEmbed from '#lib/embed/youtube.svelte'
	import { setupViewTransition } from '#lib/utils/index.js'

	import '../app.css'

	setupViewTransition()

	let { children } = $props()
</script>

<LiteYouTubeEmbed />

<div class="min-h-full pt-6">
	<Header />
	<div>
		{@render children?.()}
		<Footer />
	</div>
</div>
