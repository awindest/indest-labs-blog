<script>
	import { fade } from 'svelte/transition'

	let { url, children } = $props();
</script>

{#key url}
	<div class="transition" in:fade>
		{@render children?.()}
	</div>
{/key}

<style>
	.transition {
		height: 100%;
	}
</style>
