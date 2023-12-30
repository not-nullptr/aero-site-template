<div class="nav">
	<div class="nav__content">
		<slot />
	</div>
</div>

<style>
	.nav {
		height: 64px;
		background: url($lib/assets/wave.jpg);
		position: fixed;
		width: 100%;
		top: 0;
		left: 0;
		mask-image: linear-gradient(to bottom, rgba(0, 0, 0, 1) 80%, rgba(0, 0, 0, 0));
		-webkit-mask-image: linear-gradient(to bottom, rgba(0, 0, 0, 1) 80%, rgba(0, 0, 0, 0));
		display: flex;
		align-items: center;
		box-sizing: border-box;
	}
	.nav__content {
		width: 100%;
		max-width: 1000px;
		min-width: 1000px;
		margin-left: auto;
		margin-right: auto;
		display: flex;
		align-items: center;
		box-sizing: border-box;
		padding-bottom: 6px;
	}
</style>
