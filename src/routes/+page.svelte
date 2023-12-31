<script lang="ts">
	import DiscordWidget from "../components/DiscordWidget.svelte";
	import Header from "../components/Header.svelte";
</script>

<div class="content__container">
	<div class="top">
		<Header
			headers={[
				{
					header: "no waaaayyy :0",
					text: "thbis is. neptune's friend cat. c at. cooool cat !!1 :3",
					alt: "cat",
					src: "https://cdn.discordapp.com/attachments/1173366500149690421/1190491200428458014/135_0017.JPG",
				},
				{
					header: "woahgggg bad boy",
					text: "this cat (PICTUR ED) has been   VER ybad. he has said: FLIP.",
					alt: "cat",
					src: "https://cdn.discordapp.com/attachments/1173366500149690421/1190492467909365780/cta_kity_1.png",
				},
			]}
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
		<h1>Observe Me You Stalker</h1>
		<DiscordWidget userId="526468680498806786" />
	</div>
</div>
<div class="content__grid">
	<div class="content__item">
		<h1>Some sample text.</h1>
		<div>
			Lorem ipsum dolor sit amet consectetur adipisicing elit. Maxime id ipsa reiciendis sed
			omnis quod quis inventore. Doloremque officiis eos facilis ab quisquam id eligendi ex
			velit adipisci? Repudiandae, fugiat.
		</div>
	</div>
	<div class="content__item">
		<h1>More sample text.</h1>
		<div>
			Lorem ipsum dolor sit amet consectetur adipisicing elit. Nulla beatae repellat quam
			perspiciatis. Eligendi nulla unde cumque facere porro nam, commodi quam natus
			dignissimos, eos ab aut aspernatur rerum nostrum.
		</div>
	</div>
	<div class="content__item">
		<h1>Why You Should Give Me AWM</h1>
		<div>
			Lorem ipsum dolor sit amet consectetur adipisicing elit. Facere nesciunt quisquam
			voluptates itaque voluptas dicta minus. Quasi modi in placeat ullam at aliquam voluptate
			non laboriosam illum impedit. Libero, perferendis?
		</div>
	</div>
</div>

<style>
	.content__grid {
		margin-top: 32px;
		display: grid;
		grid-template-columns: auto auto auto;
		width: 100%;
		box-sizing: border-box;
		padding: 0px 32px;
		gap: 96px;
	}
	.content__item {
		padding: 12px 16px;
		backdrop-filter: blur(3px);
		border-radius: 4px;
		background-color: #cce1f35a;
		box-shadow:
			0px 0px 8px 0px rgba(0, 0, 0, 0.478),
			var(--aero-outline);
		text-shadow:
			0px 0px 16px #ffffff80,
			0px 0px 16px #ffffff80,
			0px 0px 16px #ffffff80,
			0px 0px 16px #ffffff80,
			0px 0px 16px #ffffff80,
			0px 0px 16px #ffffff80,
			0px 0px 16px #ffffff80;
	}
	.content__item > h1:first-child {
		margin-top: 0px;
	}
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
