<script lang="ts">
	import loading from "$lib/assets/loading.gif";
	import { onMount } from "svelte";
	import Header from "../components/Header.svelte";
	async function fetchCats(): Promise<
		{ url: string; id: string; width: number; height: number }[]
	> {
		const request = await fetch("https://api.thecatapi.com/v1/images/search?limit=10");
		const data = await request.json();
		return data;
	}
	let cats: Awaited<ReturnType<typeof fetchCats>> = [];
	onMount(async () => {
		const catsData = await fetchCats();
		cats = catsData;
	});
</script>

<div class="content__container">
	<div class="top">
		<Header
			headers={cats.length === 0
				? [
						{
							alt: "Loading...",
							header: "Loading...",
							src: loading,
							text: "The slideshow is loading. Please wait...",
						},
					]
				: cats.map((c) => ({
						header: `Cat ID ${c.id}`,
						text: `This is a cat. It's ${c.width}x${c.height} pixels.`,
						alt: `Cat ID ${c.id}`,
						src: c.url,
					}))}
		/>
		<div class="page__content">
			thanks for checking this shit out. heres some cool stuff i've done to make your life
			just a bit easier:
			<ul>
				<li>
					native-looking buttons: <button
						on:click={() => alert("why :(")}
						style="margin-left: 6px">don't click me</button
					>
				</li>
				<li>
					native-looking h1s: <h1 style="display: inline; margin-left: 4px">
						h1 type ting
					</h1>
				</li>
				<li>
					native-looking links: <a href="##" on:click={() => alert(":(")}
						>don't click me</a
					>
				</li>
			</ul>
			that's it. to be honest i dont know why i thought that stuff would help too much. anyway
			this page is pretty easily modifiable; the sidebar and stuff should all stay properly positioned
			no matter what.
		</div>
	</div>
	<div class="sidebar">
		<h1>Sidebar Header</h1>
		<p>
			You know what would go hard here? An RSS feed. Or something like that. If you're down to
			SSR your homepage (for some godforsaken reason) you could also setup some cool ass
			live-updating status thing for your Discord account, in the style of Windows Live
			Messenger.
		</p>
		<h1>God Fucking Damnit</h1>
		<p>3:30am development goes hard</p>
	</div>
</div>

<style>
	.content__container {
		display: flex;
	}
	.top {
		display: flex;
		flex-direction: column;
	}
	.sidebar {
		margin-left: 24px;
		margin-top: 16px;
		height: fit-content;
		color: #444;
	}
	.sidebar h1 {
		margin: 4px 0px;
		color: #f4793a;
		font-size: 18px;
	}
	.page__content {
		margin-top: 8px;
	}
</style>
