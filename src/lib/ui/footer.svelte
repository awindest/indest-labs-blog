<script lang="ts">
	import { GitHub, RSS } from '#lib/icons/index.js'
	import * as config from '#lib/site/config.js'
</script>

<footer
	class="mt-8 flex w-full flex-wrap justify-center gap-x-16 gap-y-6 rounded-t-2xl border border-menu-border bg-footer px-6 py-5 text-sm min-[600px]:px-12"
>
	<div class="follow">
		<p class="font-bold">Follow</p>
		<ul class="mt-2 flex flex-col gap-2 [&_a]:flex [&_a]:items-center [&_a]:gap-2">
			<li>
				<a
					href={config.github}
					target="_blank"
					rel="noreferrer"
					class="text-footer-fg hover:text-primary"
				>
					<GitHub width={18} height={18} aria-hidden={true} />
					<span>GitHub</span>
				</a>
			</li>
			<li>
				<a href="/rss.xml" target="_blank" class="text-footer-fg hover:text-primary">
					<RSS width={18} height={18} aria-hidden={true} />
					<span>RSS</span>
				</a>
			</li>
		</ul>
	</div>

	<div class="other">
		<p class="font-bold">Other</p>
		<ul class="mt-2 flex flex-col gap-2">
			<li>
				<a href="/about" class="text-footer-fg hover:text-primary">About</a>
			</li>
			<li>
				<a
					href={config.uses}
					target="_blank"
					rel="noreferrer"
					class="text-footer-fg hover:text-primary"
				>
					Uses
				</a>
			</li>
		</ul>
	</div>
</footer>
