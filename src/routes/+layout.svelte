<script lang="ts">
	import "$lib/css/index.css";
	import Footer from "../components/Footer.svelte";
	import Logo from "../components/Logo.svelte";
	import Navbar from "../components/Navbar.svelte";
</script>

<Navbar>
	<Logo topText="Windows Live™" bottomText="Essentials" />
	<div class="nav__items">
		<a href="##">Home</a>
		<a href="##">About</a>
		<a href="##">Contact</a>
		<a href="##">Blog</a>
		<a href="##">Projects</a>
		<a href="##">Resume</a>
		<a href="##">Links</a>
	</div>
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
	.nav__items {
		margin-left: 12px;
	}
	.nav__items a {
		margin-left: 12px;
		color: black;
		text-shadow:
			0px 0px 8px white,
			0px 0px 8px white,
			0px 0px 8px white,
			0px 0px 16px white;
	}
</style>
