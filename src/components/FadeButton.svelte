<script lang="ts">
	import { onMount } from "svelte";

	export let button: string;
	export let className: string = "";
	let [def, hover, active] = ["", "", ""];
	let loadedButtons: string[] = [];
	export let onLoad: () => void = () => {};
	$: (() => {
		import(`$lib/assets/buttons/${button}/default.png`).then((d) => (def = d.default));
		import(`$lib/assets/buttons/${button}/hover.png`).then((d) => (hover = d.default));
		import(`$lib/assets/buttons/${button}/active.png`).then((d) => (active = d.default));
		if (loadedButtons.length === 3) {
			onLoad();
			loadedButtons = [];
		}
	})();
</script>

{#key [def, hover, active, button]}
	<!-- svelte-ignore a11y-click-events-have-key-events -->
	<div on:click tabindex={0} role="button" class={`button__container ${className || ""}`}>
		<img
			on:load={() => (loadedButtons = [...loadedButtons, "active"])}
			class="button__active"
			src={active}
			alt="active"
		/>
		<img
			on:load={() => (loadedButtons = [...loadedButtons, "hover"])}
			class="button__hover"
			src={hover}
			alt="hover"
		/>
		<img
			on:load={() => (loadedButtons = [...loadedButtons, "default"])}
			class="button__default"
			src={def}
			alt="default"
		/>
	</div>
{/key}

<style>
	.button__container > img {
		font-size: 0;
	}
	.button__container {
		width: fit-content;
		position: relative;
	}
	.button__hover,
	.button__active {
		position: absolute;
		top: 0;
		left: 0;
		opacity: 0;
	}

	.button__default,
	.button__hover,
	.button__active {
		transition: opacity 0.25s steps(4, end);
		pointer-events: none;
	}
	.button__default {
		z-index: 0;
		position: relative;
	}
	.button__hover {
		z-index: 5;
	}
	.button__active {
		z-index: 10;
	}

	.button__container:hover .button__hover {
		opacity: 1;
	}
	.button__container:active .button__active {
		opacity: 1;
	}
	.button__container:active .button__hover {
		opacity: 0;
	}
</style>
