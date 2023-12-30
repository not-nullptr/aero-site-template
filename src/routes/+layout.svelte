<script lang="ts">
	import "$lib/css/index.css";
	import Footer from "../components/Footer.svelte";
	import Logo from "../components/Logo.svelte";
	import Navbar from "../components/Navbar.svelte";
</script>

<Navbar>
	<Logo topText="Windows Live™" bottomText="Essentials" />
</Navbar>

<div class="content">
	<slot />
	<Footer />
</div>

<style>
	.content {
		box-sizing: border-box;
		padding-top: 44px;
		margin: 8px;
		width: 100vw;
		max-width: 1000px;
		min-width: 1000px;
		margin-left: auto;
		margin-right: auto;
	}
</style>
