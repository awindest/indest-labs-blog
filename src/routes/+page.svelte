<script lang="ts">
	import { formatDate } from '#lib/utils'

	import { ArrowRight } from '#lib/icons/index.ts'
	import * as config from '#lib/site/config'

	import { tick } from 'svelte'

	let { data } = $props()
	const { posts } = $derived(data)

	const PER_PAGE = 6 // number of posts on a page

	// 1. Declare fine-grained reactive state
	let visibleCount: number = $state(PER_PAGE)
	let scrollAnchor: HTMLDivElement | undefined = $state()

	// 2. Compute reactive derivations automatically
	let visiblePosts = $derived(posts.slice(0, visibleCount))
	let hasMore = $derived(visibleCount < posts.length)

	async function loadMore() {
		// click the load more button
		visibleCount += PER_PAGE

		// Wait for Svelte 5 to finish injecting the new post components into the DOM
		await tick()

		// Smoothly roll the viewport down to bring the new posts into view
		if (scrollAnchor) {
			scrollAnchor.scrollIntoView({
				behavior: 'smooth',
				block: 'start'
			})
		}
	}

	// special effects
	const cycle = 18 // must match the shimmer animation duration (seconds)

	function spreadDelays(n: number) {
		const slot = cycle / n
		const slots = Array.from({ length: n }, (_, i) => i)

		// Fisher-Yates shuffle so slot order isn't top-to-bottom
		for (let i = n - 1; i > 0; i--) {
			const j = Math.floor(Math.random() * (i + 1))
			;[slots[i], slots[j]] = [slots[j], slots[i]]
		}

		// jitter within the first half of each slot so neighbors never touch
		return slots.map((s) => -(s * slot + Math.random() * slot * 0.5).toFixed(2))
	}

	const delays = $derived(spreadDelays(data.posts.length))
</script>

<svelte:head>
	<title>{config.title}</title>

	<meta content={config.siteDescription} name="description" />

	<meta content={config.title} property="og:title" />
	<meta content={config.siteImage} property="og:image" />
	<meta content={config.siteUrl} property="og:url" />
	<meta content={config.siteDescription} property="og:description" />
	<meta content={config.siteName} property="og:site_name" />

	<meta content={config.twitterHandle} name="twitter:creator" />
	<meta content="summary_large_image" name="twitter:card" />
	<meta content={config.title} name="twitter:title" />
	<meta content={config.siteDescription} name="twitter:description" />
	<meta content={config.siteImage} name="twitter:image" />
</svelte:head>

<!-- <main class="flex min-h-screen w-full items-center justify-center"> -->
<!-- <main class="mx-auto w-full max-w-shell px-4"> -->
<main class="mx-auto w-full max-w-shell px-4 py-8">
	<section>
		<ul class="posts">
			{#each visiblePosts as post, i}
				<li class="post sparkly-outline-effect">
					<a href={post.slug} class="title"
						>{post.title}
						<p class="date">{formatDate(post.published)}</p>
						<p class="description">{post.description}</p>
					</a>
					<svg class="sparkly-outline-container">
						<rect
							pathLength="100"
							stroke-linecap="round"
							class="sparkly-outline-blur"
							x="0"
							y="0"
							width="100%"
							height="100%"
						></rect>
						<rect
							pathLength="100"
							stroke-linecap="round"
							class="sparkly-outline-line"
							x="0"
							y="0"
							width="100%"
							height="100%"
						></rect>
					</svg>
					<div class="shimmer" style="--delay: {delays[i]}s"></div>
				</li>
			{/each}
		</ul>

		{#if hasMore}
			<!-- <div class="load-more flex justify-center py-8"> -->
			<div class="load-more">
				<button onclick={loadMore} class="load-more-btn"> Load More Posts </button>
			</div>
		{/if}
	</section>
</main>

<!-- The scroll anchor sits exactly where the next batch begins -->
<div bind:this={scrollAnchor} class="scroll-anchor"></div>

<style>
	.scroll-anchor {
		/* Safe offset to prevent scrolling directly onto the text edge */
		scroll-margin-top: 2rem;
	}
	.load-more {
		display: flex;
		justify-content: center;
		margin-top: 2rem;
	}
	.load-more-btn {
		padding: 0.75rem 2rem;
		font-size: 1rem;
		cursor: pointer;
		border-radius: 4px;
		border: 1px solid #ccc;
		background: #fff;
		transition: background 0.2s;
	}
	.load-more-btn:hover {
		background: #f5f5f5;
	}

	.posts {
		--chrome: 10rem; /* header + top padding + main's py-8; tweak to taste */
		display: grid;
		grid-template-columns: 1fr;
		gap: 1rem;
	}

	@media (min-width: 48rem) {
		.posts {
			grid-template-columns: repeat(2, 1fr);
		}
	}

	@media (min-width: 64rem) {
		.posts {
			grid-template-columns: repeat(3, 1fr);
			grid-template-rows: repeat(2, 1fr);
			min-height: calc(100dvh - var(--chrome));
		}
	}

	/* let each card stretch, and make the whole card clickable */
	.post {
		display: flex;
		container-type: inline-size; /* makes 1cqi = 1% of the card's width */
	}
	.post a {
		flex: 1;
		display: flex;
		flex-direction: column;
		justify-content: center; /* centre the text block vertically in the card */
	}
	.title {
		font-size: clamp(1.25rem, 7cqi, 2.5rem);
		line-height: 1.2;
	}

	.date {
		font-size: clamp(0.8rem, 3.5cqi, 1.1rem);
		margin-top: 0.5em;
	}

	.description {
		font-size: clamp(0.95rem, 4.5cqi, 1.35rem);
		line-height: 1.45;
		margin-top: 0.75em;
		-webkit-line-clamp: 3;
		line-clamp: 3;
	}

	@media (min-width: 64rem) {
		.description {
			-webkit-line-clamp: 8; /* desktop cards are tall, so allow more lines */
			line-clamp: 8;
		}
	}
	/* 
* There are several animations on each post card - entrance animation, 
* a random shimmer effect,
* a gradient flowing around card on hover
* and a sparkly effect or marching lines around the perimeter of each card on hover
*/
	.post {
		position: relative;

		animation:
			slide-in-right 0.5s cubic-bezier(0.25, 0, 0.3, 1),
			fade-in 0.5s cubic-bezier(0.25, 0, 0.3, 1) forwards;
		animation-timing-function: cubic-bezier(0.5, -0.1, 0.1, 1.5);

		background: var(--color-card);
		box-shadow:
			0 -1px 2px 0 hsl(220 40% 2% / 12%),
			0 2px 1px -2px hsl(220 40% 2% / 13%),
			0 5px 5px -2px hsl(220 40% 2% / 13%),
			0 10px 10px -2px hsl(220 40% 2% / 14%),
			0 20px 20px -2px hsl(220 40% 2% / 15%),
			0 40px 40px -2px hsl(220 40% 2% / 17%);

		border-radius: clamp(0px, calc((100vw - 100%) * 1e5), 5px);
		padding: 0.75rem 1rem;
	}

	.post a {
		color: var(--color-fg); /* title, readable on --color-card in both themes */
		text-decoration: none;
	}
	.post a:hover {
		color: var(--color-primary);
	}

	.date,
	.description {
		color: var(--color-card-fg); /* .date was var(--text-2) */
	}
	.post:before {
		content: '';
		background: linear-gradient(-90deg, #007cf0, #00dfd8, #ff0080, #007cf0);
		position: absolute;
		top: -2px;
		left: -2px;
		background-size: 400%;
		z-index: -1;
		filter: blur(5px);
		width: calc(100% + 4px);
		height: calc(100% + 4px);
		animation: gradient-background-anim 8s ease-in-out infinite;
		opacity: 0;
		transition: opacity 0.3s ease;
	}

	.post:hover:before {
		opacity: 1;
	}

	.post:after {
		z-index: -1;
		content: '';
		position: absolute;
		width: 100%;
		height: 100%;
		background: var(--color-card);
		left: 0;
		top: 0;
	}
	/* 
	.post:not(:last-child) {
		padding-bottom: 2rem;
	} */
	.post .shimmer {
		background: linear-gradient(
			112deg,
			transparent 30%,
			rgba(var(--geist-foreground-rgb), 0.1) 40%,
			rgba(var(--geist-foreground-rgb), 0.3) 50%,
			rgba(var(--geist-foreground-rgb), 0.1) 60%,
			transparent 80%
		);
		position: absolute;
		inset: 0;
		z-index: 20;
		pointer-events: none;
		border-radius: inherit;
		background-size: 200% auto;
		background-position: 0;
		animation: shimmer 18s cubic-bezier(0.8, 0, 0.05, 1) infinite;
		animation-delay: var(--delay);
	}
	/* add this for those people who don't want animations */
	@media (prefers-reduced-motion: no-preference) {
		animation: fade-in 0.5s ease-out;
	}
	@keyframes slide-in-right {
		0% {
			transform: translateX(-100%);
		}
	}
	@keyframes fade-in {
		from {
			opacity: 0;
		}
	}
	@keyframes shimmer {
		0% {
			opacity: 0;
		}

		5% {
			opacity: 0;
			background-position: -50%;
		}

		10% {
			opacity: 0.7;
		}

		15% {
			opacity: 0;
			background-position: 150%;
		}

		20% {
			opacity: 0;
		}

		to {
			opacity: 0;
		}
	}

	@keyframes animate {
		0% {
			background-position: 0 0;
		}

		50% {
			background-position: 400% 0;
		}

		100% {
			background-position: 0 0;
		}
	}

	@keyframes gradient-background-anim {
		50% {
			background-position: 140% 50%;
			transform: skew(-2deg);
		}
	}
	.title {
		font-size: clamp(1.75rem, 10cqi, 3.5rem);
		font-weight: 600;
		line-height: 1.15;
		text-transform: capitalize;
		text-decoration: none;
	}

	.date {
		font-size: clamp(0.9rem, 4cqi, 1.2rem);
		font-weight: 400;
		text-transform: none;
		color: var(--color-card-fg);
		margin-top: 0.5em;
	}

	.description {
		font-size: clamp(1.1rem, 5.5cqi, 1.6rem);
		font-weight: 400;
		line-height: 1.45;
		text-transform: none;
		color: var(--color-card-fg);
		margin-top: 0.75em;

		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 4;
		line-clamp: 4;
		overflow: hidden;
	}

	@media (min-width: 64rem) {
		.description {
			-webkit-line-clamp: 8; /* desktop cards are tall */
			line-clamp: 8;
		}
	}
	.sparkly-outline-effect {
		/* Kevin Powell */
		--sparkly-outline-line-color: var(--color-fg);
		--sparkly-outline-line-thickness: 2px;
		--sparkly-outline-line-length: 20px;
		--sparkly-outline-blur-color: var(--color-fg);
		--sparkly-outline-blur-size: 5px;
		--sparkly-outline-offset: 0px;
		--animation-speed: 1200ms;
		/* do not change, used for calculations */
		/* --container-offset: 100px; */
		position: relative;
	}

	.sparkly-outline-container {
		pointer-events: none;
		position: absolute;
		inset: 0;
		width: 100%;
		height: 100%;
		overflow: visible; /* let the blur bleed past the box instead of clipping */
		opacity: 0;
	}

	.sparkly-outline-blur,
	.sparkly-outline-line {
		fill: transparent;
		stroke: black;
		stroke-width: 5px;
		vector-effect: non-scaling-stroke; /* keeps stroke width constant even though preserveAspectRatio="none" scales x/y differently */
		stroke-dasharray: var(--sparkly-outline-line-length)
			calc(50px - var(--sparkly-outline-line-length));
	}

	.sparkly-outline-effect:is(:hover, :focus) :is(.sparkly-outline-line, .sparkly-outline-blur) {
		stroke-dashoffset: -80px;
		transition: stroke-dashoffset var(--animation-speed) ease-in;
	}

	.sparkly-outline-line {
		stroke: var(--sparkly-outline-line-color);
		stroke-width: var(--sparkly-outline-line-thickness);
	}

	.sparkly-outline-blur {
		filter: blur(var(--sparkly-outline-blur-size));
		stroke: var(--sparkly-outline-blur-color);
		stroke-width: var(--sparkly-outline-blur-size);
	}

	.sparkly-outline-effect:is(:hover, :focus) .sparkly-outline-container {
		animation: sparkly-outline-visibility ease-in-out var(--animation-speed);
	}

	@keyframes sparkly-outline-visibility {
		0%,
		100% {
			opacity: 0;
		}
		25%,
		75% {
			opacity: 1;
		}
	}
</style>
