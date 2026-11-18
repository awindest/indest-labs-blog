<!-- dmbok-wheel.svelte -->

<script lang="ts">
	import { tick } from 'svelte'
	import Image from '#lib/components/image.svelte'

	const VB = 1254 // SVG viewBox size

	type Hotspot = {
		name: string
		description: string
		x: number
		y: number
		shape: 'circle' | 'path'
		geometry: Record<string, string | number>
	}

	// The data that used to live in data-* attributes
	const hotspots: Hotspot[] = [
		{
			name: 'Data Governance',
			description:
				'The central discipline that establishes accountability, policies, standards, and decision rights across all data management areas.',
			x: 622,
			y: 614,
			shape: 'circle',
			geometry: { cx: 622, cy: 614, r: 234 }
		},
		{
			name: 'Data Architecture',
			description: 'The enterprise data structures, flows, and architectural blueprints.',
			x: 624,
			y: 214,
			shape: 'path',
			geometry: {
				d: 'M 465.7,99.1 A 540,540 0 0 1 790.7,103.0 L 697.8,365.3 A 260,260 0 0 0 544.1,365.9 Z'
			}
		},
		{
			name: 'Data Modeling & Design',
			description: 'Conceptual, logical, and physical models that describe data.',
			x: 862,
			y: 294,
			shape: 'path',
			geometry: {
				d: 'M 800.2,106.2 A 540,540 0 0 1 1064.9,307.0 L 832.2,461.0 A 260,260 0 0 0 707.3,368.4 Z'
			}
		},
		{
			name: 'Data Storage & Operations',
			description: 'Platforms, databases, integration, and operational data management.',
			x: 1004,
			y: 494,
			shape: 'path',
			geometry: {
				d: 'M 1070.5,315.3 A 540,540 0 0 1 1161.7,598.3 L 882.0,612.3 A 260,260 0 0 0 838.0,469.2 Z'
			}
		},
		{
			name: 'Data Security',
			description: 'Protecting data through access control, privacy, and risk management.',
			x: 1005,
			y: 730,
			shape: 'path',
			geometry: {
				d: 'M 1161.9,608.3 A 540,540 0 0 1 1067.4,921.4 L 842.1,752.4 A 260,260 0 0 0 881.9,622.3 Z'
			}
		},
		{
			name: 'Reference & Master Data',
			description: 'Consistent, trusted definitions for key entities and reference values.',
			x: 876,
			y: 923,
			shape: 'path',
			geometry: {
				d: 'M 1061.6,929.6 A 540,540 0 0 1 836.1,1111.7 L 727.1,851.8 A 260,260 0 0 0 836.6,760.8 Z'
			}
		},
		{
			name: 'Data Warehousing & Business Intelligence',
			description: 'Analytical data stores, reporting, and decision-support capabilities.',
			x: 628,
			y: 1014,
			shape: 'path',
			geometry: {
				d: 'M 826.9,1115.6 A 540,540 0 0 1 414.6,1114.6 L 540.9,861.0 A 260,260 0 0 0 717.9,855.7 Z'
			}
		},
		{
			name: 'Data Quality',
			description: 'Defining, measuring, monitoring, and improving fitness for use.',
			x: 377,
			y: 930,
			shape: 'path',
			geometry: {
				d: 'M 405.4,1110.6 A 540,540 0 0 1 184.0,931.9 L 415.2,771.5 A 260,260 0 0 0 531.4,857.7 Z'
			}
		},
		{
			name: 'Document & Content Management',
			description: 'Managing documents, records, and unstructured information.',
			x: 241,
			y: 736,
			shape: 'path',
			geometry: {
				d: 'M 178.3,923.7 A 540,540 0 0 1 82.0,610.9 L 362.1,621.9 A 260,260 0 0 0 409.3,763.5 Z'
			}
		},
		{
			name: 'Metadata Management',
			description: 'Managing data definitions, lineage, context, and technical metadata.',
			x: 240,
			y: 494,
			shape: 'path',
			geometry: {
				d: 'M 82.2,600.9 A 540,540 0 0 1 181.3,303.9 L 402.3,475.0 A 260,260 0 0 0 362.0,611.9 Z'
			}
		},
		{
			name: 'Data Lifecycle Management',
			description: 'Managing data from creation and use through retention and disposal.',
			x: 383,
			y: 293,
			shape: 'path',
			geometry: {
				d: 'M 187.2,295.8 A 540,540 0 0 1 456.1,102.1 L 534.6,369.1 A 260,260 0 0 0 407.8,466.6 Z'
			}
		}
	]

	// DOM refs (only needed for measuring)
	let stage: HTMLDivElement
	let svg: SVGSVGElement
	let tip: HTMLDivElement

	// Reactive state: the template derives every class and the popup content from these
	let activeIndex = $state<number | null>(null)
	let shown = $state<Hotspot | null>(null) // last-shown content, so text doesn't vanish mid fade-out
	let tipPos = $state({ x: 0, y: 0 })

	// Plain flag: not rendered anywhere, so it doesn't need to be reactive
	let hovering = false

	function open(i: number) {
		activeIndex = i
		shown = hotspots[i]
	}

	function hide() {
		activeIndex = null
		hovering = false
	}

	// Keep the popup inside the stage
	function place(x: number, y: number, below: boolean) {
		const sw = stage.clientWidth,
			sh = stage.clientHeight
		const tw = tip.offsetWidth,
			th = tip.offsetHeight
		let left = x - tw / 2
		let top = below ? y + 18 : y - th - 18
		left = Math.max(4, Math.min(sw - tw - 4, left))
		top = Math.max(4, Math.min(sh - th - 4, top))
		tipPos = { x: left, y: top }
	}

	// Anchor the popup to a segment (touch and keyboard focus)
	function anchor(i: number) {
		const r = svg.getBoundingClientRect(),
			s = stage.getBoundingClientRect()
		const k = r.width / VB
		const x = r.left - s.left + hotspots[i].x * k
		const y = r.top - s.top + hotspots[i].y * k
		place(x, y, y < stage.clientHeight / 2)
	}

	// Follow the mouse pointer
	function follow(e: PointerEvent) {
		const s = stage.getBoundingClientRect()
		const x = e.clientX - s.left,
			y = e.clientY - s.top
		const tw = tip.offsetWidth,
			th = tip.offsetHeight
		let left = x + 16,
			top = y + 20
		if (left + tw > stage.clientWidth - 4) left = x - tw - 16
		if (top + th > stage.clientHeight - 4) top = y - th - 16
		left = Math.max(4, left)
		top = Math.max(4, top)
		tipPos = { x: left, y: top }
	}

	// State changes reach the DOM asynchronously, so wait a tick before measuring the popup
	async function openAndAnchor(i: number) {
		open(i)
		await tick()
		anchor(i)
	}

	async function onPointerEnter(e: PointerEvent, i: number) {
		if (e.pointerType !== 'mouse') return
		hovering = true
		open(i)
		await tick()
		follow(e)
	}

	function onPointerMove(e: PointerEvent, i: number) {
		if (e.pointerType === 'mouse' && activeIndex === i) follow(e)
	}

	function onPointerLeave(e: PointerEvent) {
		if (e.pointerType === 'mouse') hide()
	}

	// Touch / pen: tap a segment to open its popup
	function onPointerDown(e: PointerEvent, i: number) {
		if (e.pointerType === 'mouse') return
		openAndAnchor(i)
	}

	function onFocus(i: number) {
		if (hovering) return
		openAndAnchor(i)
	}

	function onBlur() {
		if (!hovering) hide()
	}

	function onKeyDown(e: KeyboardEvent, i: number) {
		if (e.key === 'Enter' || e.key === ' ') {
			e.preventDefault()
			openAndAnchor(i)
		}
		if (e.key === 'Escape') {
			hide()
			;(e.currentTarget as SVGElement).blur()
		}
	}

	// Tap anywhere outside the wheel hotspots to dismiss
	function onDocumentPointerDown(e: PointerEvent) {
		if (!(e.target as Element).closest?.('.hotspot')) hide()
	}

	function onResize() {
		if (activeIndex !== null && !hovering) anchor(activeIndex)
	}
</script>

<!-- Replaces document/window addEventListener in onMount; Svelte cleans these up automatically -->
<svelte:document onpointerdown={onDocumentPointerDown} />
<svelte:window onresize={onResize} />

<div class="stage" bind:this={stage}>
	<div class="wheel">
		<img
			src="dmbok-wheel.webp"
			alt="Blue neon DAMA-DMBOK wheel with Data Governance in the center and labeled data management domains"
		/>
		<svg
			bind:this={svg}
			viewBox="0 0 1254 1254"
			aria-label="Interactive segment hotspots"
			role="group"
		>
			{#each hotspots as h, i (h.name)}
				<svelte:element
					this={h.shape}
					{...h.geometry}
					class="hotspot"
					class:active={activeIndex === i}
					tabindex="0"
					role="button"
					aria-label={h.name}
					onpointerenter={(e: PointerEvent) => onPointerEnter(e, i)}
					onpointermove={(e: PointerEvent) => onPointerMove(e, i)}
					onpointerleave={onPointerLeave}
					onpointerdown={(e: PointerEvent) => onPointerDown(e, i)}
					onfocus={() => onFocus(i)}
					onblur={onBlur}
					onkeydown={(e: KeyboardEvent) => onKeyDown(e, i)}
				/>
			{/each}
		</svg>
	</div>

	<div
		class="tip"
		class:show={activeIndex !== null}
		style:transform="translate({tipPos.x}px, {tipPos.y}px)"
		bind:this={tip}
		role="tooltip"
		aria-live="polite"
	>
		<h2>{shown?.name}</h2>
		<p>{shown?.description}</p>
	</div>
</div>

<style>
	:root {
		color-scheme: dark;
		--cyan: #35eaff;
		--blue: #1688ff;
	}

	* {
		box-sizing: border-box;
	}

	body {
		margin: 0;
		min-height: 100vh;
		text-align: center;
		padding: clamp(16px, 3vw, 36px);
		color: #eaf7ff;
		font:
			16px/1.5 system-ui,
			-apple-system,
			Segoe UI,
			sans-serif;
		background: radial-gradient(ellipse at 50% 35%, #092c67 0%, #04132f 50%, #020713 100%);
	}

	main {
		max-width: 860px;
		margin: 0 auto;
	}

	h1 {
		margin: 0 0 6px;
		font-size: clamp(1.4rem, 3vw, 2.2rem);
		letter-spacing: 0.02em;
	}

	.intro {
		margin: 0 0 22px;
		color: #a9d7ff;
	}

	/* Positioning context for the wheel and its popup (popup must not be clipped by the wheel) */
	.stage {
		position: relative;
		width: 100%;
	}

	.wheel {
		position: relative;
		width: 100%;
		aspect-ratio: 1;
		border-radius: 50%;
		overflow: hidden;
		box-shadow: 0 0 28px #087dff55;
	}

	.wheel img {
		display: block;
		width: 100%;
		height: 100%;
		object-fit: contain;
	}

	.wheel svg {
		position: absolute;
		inset: 0;
		width: 100%;
		height: 100%;
	}

	.hotspot {
		fill: #00d9ff;
		fill-opacity: 0;
		stroke: transparent;
		stroke-width: 3;
		stroke-linejoin: round;
		cursor: pointer;
		pointer-events: all;
		transition:
			fill-opacity 0.16s ease,
			stroke 0.16s ease;
		outline: none;
		-webkit-tap-highlight-color: transparent;
	}

	.hotspot:hover,
	.hotspot:focus,
	.hotspot.active {
		fill-opacity: 0.18;
		stroke: #74f7ff;
		filter: drop-shadow(0 0 8px #00cfff);
	}

	/* Popup */
	.tip {
		position: absolute;
		left: 0;
		top: 0;
		z-index: 5;
		width: max-content;
		max-width: min(280px, 80%);
		padding: 14px 16px;
		border: 1px solid #1a8bffaa;
		border-radius: 14px;
		background: #061632ef;
		box-shadow:
			0 0 22px #087dff55,
			inset 0 0 18px #087dff18;
		pointer-events: none;
		opacity: 0;
		visibility: hidden;
		transition:
			opacity 0.14s ease,
			visibility 0.14s;
	}

	.tip.show {
		opacity: 1;
		visibility: visible;
	}

	.tip h2 {
		margin: 0 0 4px;
		font-size: 1.05rem;
		line-height: 1.25;
		color: #fff;
	}

	.tip p {
		margin: 0;
		font-size: 0.9rem;
		color: #c4dff7;
	}

	footer {
		color: #7fa8ce;
		font-size: 0.8rem;
		margin-top: 20px;
	}

	@media (prefers-reduced-motion: reduce) {
		* {
			transition: none !important;
		}
	}
</style>
