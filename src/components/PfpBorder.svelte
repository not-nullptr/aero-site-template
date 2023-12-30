<script lang="ts">
	import type { DiscordStatus } from "lanyard-wrapper";

	export let pfp: string;
	export let status: DiscordStatus;

	async function getBorderFromStatus(
		status: DiscordStatus,
		type: "animated-from" | "animated-to" | "static",
	) {
		return (await import(`$lib/assets/borders/${status}-${type}.png`)).default;
	}
</script>

<div class={`border ${status}`}>
	<img src={pfp} alt="pfp" class="pfp" />
	<div class="status">
		{#await getBorderFromStatus(status, "static")}
			<div></div>
		{:then d}
			<img src={d} alt={status} />
		{/await}
	</div>
</div>

<style>
	.border {
		position: relative;
	}
	.border.offline {
		opacity: 0.75;
	}
	.pfp {
		position: absolute;
		top: 11px;
		left: 12px;
		width: 22px;
		height: 22px;
	}
</style>
