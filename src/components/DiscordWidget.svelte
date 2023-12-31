<script lang="ts">
	import { connectWebSocket } from "lanyard-wrapper";
	import type { Data } from "lanyard-wrapper";
	import { onMount } from "svelte";
	import PfpBorder from "../components/PfpBorder.svelte";
	import { ActivityType } from "discord-api-types/v9";
	import game from "$lib/assets/statuses/game.png";
	import music from "$lib/assets/statuses/music.png";

	export let userId: string;
	let userData: Data | null = null;
	let activity:
		| (Data["activities"][0] & {
				emoji?: { name: string; id: string; animated?: boolean };
		  })
		| undefined = undefined;
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
	function lanyardUpdate(data: Data) {
		userData = data;
		activity = findActivity(data);
	}

	onMount(() => {
		let ws: any = null;
		connectWebSocket(userId, lanyardUpdate).then((w) => (ws = w));
		return () => ws?.close();
	});
</script>

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
							{#if activity.details}<span>(<i>{activity.details}</i>)</span>{/if}
						</div>
					{:else if activity.type === ActivityType.Custom}
						<div class="activity__text">
							{#if !!activity.emoji}
								{#if activity.emoji.id}
									<img
										src={`https://cdn.discordapp.com/emojis/${activity.emoji.id}.webp?size=24`}
										alt={activity.emoji.name}
										class="activity__emoji"
									/>
								{:else}
									{activity.emoji.name}
								{/if}
							{/if}
							{#if activity.state}<span>{activity.state}</span>{/if}
						</div>
					{/if}
				{/if}
			</div>
		</div>
	</div>
	<div class="footer">
		This is a live-updating widget of {userData.discord_user.display_name ||
			userData.discord_user.username}'s status on Discord.
		<br />
		Powered by
		<a class="footer__link" target="_blank" href="https://lanyard.eggsy.xyz/">Lanyard</a>.
	</div>
{/if}

<style>
	:global(.play__button) {
		margin-left: -4px;
		margin-right: -4px;
	}
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
		margin-top: 4px;
	}
	.aero__user__content > div:last-child {
		color: #888;
	}
	.activity__emoji {
		width: 13px;
		height: 13px;
		margin-top: 0px;
		margin-bottom: -1px;
	}
	.footer {
		margin-top: 4px;
	}
	.footer,
	.footer * {
		opacity: 0.5;
		font-size: 10px;
	}
	.footer__link {
		color: #000000;
		opacity: 1;
	}
</style>
