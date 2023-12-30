<script lang="ts">
	import { connectWebSocket } from "lanyard-wrapper";
	import type { Data } from "lanyard-wrapper";
	import loading from "$lib/assets/loading.gif";
	import { onMount } from "svelte";
	import Header from "../components/Header.svelte";
	import { PUBLIC_USER_ID } from "$env/static/public";
	import PfpBorder from "../components/PfpBorder.svelte";
	import { ActivityType } from "discord-api-types/v9";
	import game from "$lib/assets/statuses/game.png";
	import music from "$lib/assets/statuses/music.png";

	let cats: Awaited<ReturnType<typeof fetchCats>> = [];
	let userData: Data | null = null;
	let activity: Data["activities"][0] | undefined = undefined;

	function lanyardUpdate(data: Data) {
		userData = data;
		activity = findActivity(data);
	}

	async function fetchCats(): Promise<
		{ url: string; id: string; width: number; height: number }[]
	> {
		const request = await fetch("https://api.thecatapi.com/v1/images/search?limit=10");
		const data = await request.json();
		return data;
	}
	function findActivity(data: Data) {
		const priorityOrder = [
			ActivityType.Listening,
			ActivityType.Streaming,
			ActivityType.Playing,
			ActivityType.Custom,
		];

		const sortedActivities = data.activities.sort((a, b) => {
			return priorityOrder.indexOf(a.type) - priorityOrder.indexOf(b.type);
		});

		return sortedActivities.find(
			(a) =>
				a.type === ActivityType.Listening ||
				a.type === ActivityType.Streaming ||
				a.type === ActivityType.Playing ||
				a.type === ActivityType.Custom,
		);
	}
	onMount(() => {
		fetchCats().then((c) => (cats = c));
		let ws: any = null;
		connectWebSocket(PUBLIC_USER_ID, lanyardUpdate).then((w) => (ws = w));
		return () => ws?.close();
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
		{#if typeof userData !== undefined && userData !== null}
			<div class="aero__user">
				<PfpBorder
					pfp={`https://cdn.discordapp.com/avatars/${userData.discord_user.id}/${userData.discord_user.avatar}.webp?size=64`}
					status={userData.discord_status}
				/>
				<div class="aero__user__content">
					<div>
						{userData.discord_user.display_name || userData.discord_user.username}
					</div>
					<div class="activity__status">
						{#if activity !== undefined}
							{#if activity.type === ActivityType.Listening}
								<img class="activity__icon" src={music} alt="Music Icon" />
								<div class="activity__text">
									<span>{activity.state} - {activity.details}</span>
								</div>
							{:else if activity.type === ActivityType.Streaming}
								<img class="activity__icon" src={game} alt="Streaming Icon" />
								<div class="activity__text">
									Streaming "<i><b>{activity.name}</b></i>"
								</div>
							{:else if activity.type === ActivityType.Playing}
								<img class="activity__icon" src={game} alt="Playing Icon" />
								<div class="activity__text">
									Playing <b>{activity.name}</b>
									{#if activity.details}<span>(<i>{activity.details}</i>)</span
										>{/if}
								</div>
							{:else if activity.type === ActivityType.Custom}
								<div>d</div>
							{/if}
						{/if}
					</div>
				</div>
			</div>
		{/if}
	</div>
</div>

<style>
	.activity__status {
		display: flex;
		margin-bottom: 3px;
		margin-top: 1px;
		align-items: center;
	}
	.activity__icon {
		margin-right: 4px;
		width: 13px;
		height: 13px;
		min-width: 13px;
		min-height: 13px;
		max-width: 13px;
		max-height: 13px;
	}
	.aero__user {
		margin-top: 8px;
		display: flex;
		align-items: center;
		max-width: 300px;
		background: linear-gradient(to bottom, rgb(251, 251, 251), rgb(228, 228, 228));
		box-shadow:
			inset 0px 0px 0px 1px rgba(0, 0, 0, 0.333),
			inset 0px 0px 0px 2px white;
		border-radius: 4px;
		padding-right: 8px;
	}
	.aero__user * {
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}
	.aero__user__content {
		margin-bottom: 4px;
		margin-left: 2px;
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
	.aero__user__content > div:last-child {
		color: #888;
	}
</style>
