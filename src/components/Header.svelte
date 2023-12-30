<script lang="ts">
	import { onMount } from "svelte";
	import FadeButton from "./FadeButton.svelte";
	import loading from "$lib/assets/loading.gif";
	import frame from "$lib/assets/slideshow/frame.png";
	import playlist from "$lib/assets/audio/FA-playlist.mp3";

	let currentIndex = 0;
	let paused = true;
	let muted = true;
	const volume = 0.3;
	let buttons: string[] = [];
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
	let audio: HTMLAudioElement | null;

	function fadeAudioIn() {
		if (!audio) return;
		audio.play();
		let interval = setInterval(() => {
			if (!audio) return;
			if (audio.volume < volume) {
				audio.volume = Math.min(audio.volume + 0.01, volume);
			} else {
				audio.volume = volume;
				clearInterval(interval);
			}
		}, 10);
	}

	function fadeAudioOut() {
		if (!audio) return;
		let interval = setInterval(() => {
			if (!audio) return;
			if (audio.volume > 0) {
				audio.volume = Math.max(audio.volume - 0.01, 0);
			} else {
				audio.volume = 0;
				clearInterval(interval);
			}
		}, 10);
	}

	function nextHeader() {
		reSetTimeout();
		currentIndex = (currentIndex + 1) % headers.length;
	}

	function previousHeader() {
		reSetTimeout();
		currentIndex = (currentIndex - 1 + headers.length) % headers.length;
	}

	function reSetTimeout() {
		paused = false;
		clearInterval(interval);
		interval = setInterval(() => {
			nextHeader();
		}, 5000);
	}
	function reClearTimeout() {
		paused = true;
		clearInterval(interval);
	}
	async function preLoadImages() {
		const glob = import.meta.glob("$lib/assets/buttons/**/*.png");
		const images = (await Promise.all(Object.values(glob).map((v) => v()))).map(
			(v) => (v as { default: string }).default,
		);
		buttons = images;
	}
	let loaded = 0;
	function incrementLoaded() {
		loaded++;
		if (loaded === headers.length) {
		}
	}
	function muteToggle() {
		muted = !muted;
		muted ? fadeAudioOut() : fadeAudioIn();
	}
	function mute() {
		if (muted) return;
		muted = true;
		fadeAudioOut();
	}
	function unmute() {
		if (!muted) return;
		muted = false;
		fadeAudioIn();
	}
	onMount(() => {
		audio = document.querySelector("audio");
		if (audio) {
			audio.volume = volume;
		}
		preLoadImages();
		return reClearTimeout;
	});
</script>

<svelte:head>
	{#each buttons as button}
		<link rel="preload" href={button} as="image" />
	{/each}
	{#each headers as header}
		<link rel="preload" href={header.src} as="image" />
	{/each}
</svelte:head>

<!-- svelte-ignore a11y-no-static-element-interactions -->
<div class="top__container">
	<audio src={playlist} />
	<div class="preload__container">
		<!-- {#each headers as header}
			<img src={header.src} alt={header.alt} />
		{/each} -->
	</div>
	<div class="header__container">
		<div class="header__gradient" />
		{#if loaded > 2}
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
		{:else}
			<div class="header__content">
				<div>
					<h1>Loading...</h1>
					<div>Your slideshow is loading. Please wait...</div>
				</div>
			</div>
		{/if}
	</div>
	<div class="img__container">
		<div class="hover__outline" />
		<div class="slideshow__buttons">
			<div class="mute__button">
				{#if muted}
					<FadeButton on:click={muteToggle} button="mute" />
				{:else}
					<FadeButton on:click={muteToggle} button="audible" />
				{/if}
			</div>
			<div style="display: flex; align-items: center; position: relative;">
				<img src={frame} alt="frame" class="buttons__frame" />
				<FadeButton
					onLoad={incrementLoaded}
					on:click={() => {
						previousHeader();
						unmute();
					}}
					button="left"
				/>
				<FadeButton
					onLoad={incrementLoaded}
					on:click={() => {
						if (paused) {
							unmute();
							reSetTimeout();
						} else {
							mute();
							reClearTimeout();
						}
					}}
					className="play__button"
					button={paused ? "play" : "pause"}
				/>
				<FadeButton
					onLoad={incrementLoaded}
					on:click={() => {
						nextHeader();
						unmute();
					}}
					button="right"
				/>
			</div>
		</div>
		<div class="img">
			{#if loaded > 2}
				{#each headers as header, index}
					<img
						src={header.src}
						alt={header.alt}
						class={index === currentIndex ? "fade-in" : "fade-out"}
					/>
				{/each}
			{:else}
				<img src={loading} alt="Loading" />
			{/if}
		</div>
	</div>
</div>

<style>
	.preload__container {
		position: absolute;
		top: 0;
		left: 0;
		opacity: 0;
		pointer-events: none;
		width: 1px;
		height: 1px;
	}
	/* .preload__container > img {
		width: 1px;
		height: 1px;
		opacity: 0;
	} */
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
	.mute__button {
		position: absolute;
		top: 4px;
		right: 4px;
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
		align-items: flex-end;
		justify-content: center;
		opacity: 0.5;
		transition: steps(4, end) opacity 0.25s;
		z-index: 99;
	}
	:global(.button__container) {
		z-index: 20;
	}
	.top__container:hover .slideshow__buttons {
		opacity: 1;
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
	.top__container:hover .hover__outline {
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
	.buttons__frame {
		position: absolute;
		bottom: 6px;
		left: -13px;
		width: 172px;
		height: auto;
		opacity: 0;
	}
</style>
