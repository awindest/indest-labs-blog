<script lang="ts">
	import { Bluesky, GitHub, RSS, X, YouTube } from '#lib/icons/index.js';
	import * as config from '#lib/site/config.js';
</script>

<div class="hidden items-center gap-4 min-[768px]:flex">
	<a href={config.repository} target="_blank" rel="noreferrer">
		<GitHub width={24} height={24} aria-label="GitHub repository" viewBox="0 0 22 24" />
	</a>
	<a href="/rss.xml" target="_blank">
		<RSS width={24} height={24} aria-label="RSS feed" />
	</a>
</div>
