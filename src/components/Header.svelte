<script lang="ts">
	import { onMount } from "svelte";

	let currentIndex = 0;
	export let headers: {
		header: string;
		text: string;
		src: string;
		alt: string;
	}[] = [
		{
			header: "Sample Header 1",
			text: "This slide demonstrates an example of fading.",
			src: "https://via.placeholder.com/285x200",
			alt: "Placeholder 1",
		},
		{
			header: "Sample Header 2",
			text: "This slide also demonstrates yet another example of fading.",
			src: "https://via.placeholder.com/300x200",
			alt: "Placeholder 2",
		},
		// Add more headers as needed
	];
	let interval: number;
	function nextHeader() {
		currentIndex = (currentIndex + 1) % headers.length;
	}

	function previousHeader() {
		currentIndex = (currentIndex - 1 + headers.length) % headers.length;
	}

	function reSetTimeout() {
		console.log("set");
		clearInterval(interval);
		interval = setInterval(() => {
			nextHeader();
		}, 5000);
	}
	function reClearTimeout() {
		console.log("clear");
		clearInterval(interval);
	}
	onMount(() => {
		reSetTimeout();
		return reClearTimeout;
	});
</script>

<div class="top__container">
	<div class="header__container">
		<div class="header__gradient" />
		{#each headers as header, index}
			<div class="header__content {index === currentIndex ? 'fade-in' : 'fade-out'}">
				<div>
					<h1>{header.header}</h1>
					<div class={index === currentIndex ? "fade-in" : "fade-out"}>
						{header.text}
					</div>
				</div>
			</div>
		{/each}
	</div>
	<div class="img__container">
		<!-- svelte-ignore a11y-click-events-have-key-events -->
		<div class="hover__outline" />
		<!-- svelte-ignore a11y-click-events-have-key-events -->
		<!-- svelte-ignore a11y-no-static-element-interactions -->
		<div class="slideshow__buttons" on:mouseenter={reClearTimeout} on:mouseleave={reSetTimeout}>
			<!-- svelte-ignore a11y-no-static-element-interactions -->
			<!-- svelte-ignore a11y-click-events-have-key-events -->
			<!-- svelte-ignore a11y-no-static-element-interactions -->
			<div class="slideshow__button left" on:click={previousHeader}>◀</div>
			<!-- svelte-ignore a11y-no-static-element-interactions -->
			<!-- svelte-ignore a11y-click-events-have-key-events -->
			<div class="slideshow__button right" on:click={nextHeader}>▶</div>
		</div>
		<div class="img">
			{#each headers as header, index}
				<img
					src={header.src}
					alt={header.alt}
					class={index === currentIndex ? "fade-in" : "fade-out"}
				/>
			{/each}
		</div>
	</div>
</div>

<style>
	.header__content {
		transition: opacity 0.25s steps(4, end);
		position: relative;
		padding: 8px;
		grid-row-start: 1;
		grid-column-start: 1;
	}

	.header__content.fade-out {
		opacity: 0;
	}

	.header__content.fade-in {
		opacity: 1;
		position: relative;
	}

	.img img {
		transition: opacity 0.25s steps(4, end);
		max-width: 100%;
		height: auto;
	}

	.img img.fade-out {
		opacity: 0;
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
	}

	.img img.fade-in {
		opacity: 1;
		position: relative;
	}
	.top__container {
		width: fit-content;
		display: flex;
		align-items: center;
		width: 628px;
	}
	.top__container *:not(.header__gradient) {
		z-index: 1;
	}
	.img__container {
		position: relative;
		margin-top: 24px;
		border-top-right-radius: 4px;
		border-bottom-right-radius: 4px;
		width: 285px;
		height: 200px;
		overflow: hidden;
	}
	.img__container > .slideshow__buttons {
		position: absolute;
		top: 0;
		left: 0;
		width: calc(100% + 1px);
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: space-between;
		opacity: 0.5;
		transition: steps(4, end) opacity 0.25s;
		z-index: 99;
	}
	.img__container:hover > .slideshow__buttons > .slideshow__button {
		background-color: rgba(0, 0, 0, 0.25);
	}
	.img__container:hover > .slideshow__buttons {
		opacity: 1;
	}
	.slideshow__button {
		width: 32px;
		height: 72px;
		background-color: rgba(98, 98, 98, 0.75);
		box-shadow: var(--aero-outline);
		color: rgba(255, 255, 255, 0.729);
		display: flex;
		align-items: center;
		box-sizing: border-box;
		padding: 0px 8px;
		backdrop-filter: blur(3px);
		cursor: pointer;
		transition: steps(4, end) background-color 0.25s;
	}
	.slideshow__button:hover {
		filter: brightness(1.1);
	}
	.slideshow__button:active {
		filter: brightness(0.9);
	}
	.slideshow__button.left {
		border-top-right-radius: 4px;
		border-bottom-right-radius: 4px;
	}
	.slideshow__button.right {
		border-top-left-radius: 4px;
		border-bottom-left-radius: 4px;
		padding: 0 12px;
	}
	.img {
		width: 100%;
		height: 100%;
	}
	.img > img {
		object-fit: cover;
		object-position: center center;
		width: 100%;
		height: 100%;
	}
	.hover__outline {
		box-shadow:
			inset 0px 0px 0px 1px rgba(0, 0, 0, 0.563),
			inset 0px 0px 0px 2px rgba(255, 255, 255, 0.659);
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		pointer-events: none;
		opacity: 0;
		transition: 0.25s opacity steps(4, end);
		border-top-right-radius: 4px;
		border-bottom-right-radius: 4px;
		z-index: 9999 !important;
	}
	.img__container:hover .hover__outline {
		opacity: 1;
	}
	/* hack for fading; not ideal but eh */
	.header__gradient {
		position: absolute;
		top: 0;
		left: 0;
		height: 100%;
		width: 100%;
		background: linear-gradient(to right, #fcfcfc, transparent 30%, transparent 70%, #fcfcfc);
		z-index: 0;
	}
	.header__container {
		z-index: 5 !important;
		position: relative;
		display: grid;
		grid-template-columns: 1fr;
		align-items: center;
		width: 360px;
		height: 200px;
		margin-top: 24px;
		border-top: solid thin #eaeef1;
		border-bottom: solid thin #eaeef1;
		box-shadow:
			inset 0px 20px 10px -10px #f3f8fb,
			inset 0px -20px 10px -10px #f3f8fb;
		box-sizing: border-box;
		padding: 0px 24px;
	}
	.header__content h1 {
		font-size: 20px;
		font-weight: 600;
		margin-bottom: 12px;
		margin-top: -8px;
	}
	.header__content > div {
		color: #444;
	}
</style>
