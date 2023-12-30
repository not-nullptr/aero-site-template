<script lang="ts">
	export let topText = "Windows Live™";
	export let bottomText = "Essentials";
</script>

<div class="logo">
	<div class="logo__image" />
	<div class="logo__text">
		<div class="text__top">
			{topText}
		</div>
		<div class="text__bottom">
			{bottomText}
		</div>
	</div>
</div>

<style>
	.logo {
		display: flex;
		align-items: center;
	}
	.logo__image {
		margin-right: 6px;
		background-image: url($lib/assets/controls-sheet.png);
		width: 18px;
		height: 16px;
	}
	.text__top {
		font-size: 12px;
	}
	.text__bottom {
		font-size: 18px;
		margin-top: -3px;
	}
</style>
