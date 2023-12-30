<div class="seperator" />
<div class="footer">
	<div class="left">
		<a href="##">© 2009 - {new Date().getFullYear()} nullptr and neptune</a>
	</div>
	<div class="right">
		<a href="https://aerochat.live" target="_blank">Download AeroChat</a>
		<a href="##">Feedback</a>
	</div>
</div>

<style>
	.seperator {
		width: 100%;
		background-color: #ccc;
		height: 1px;
		margin-top: 64px;
	}
	.footer {
		padding: 8px 2px;
	}
	.left *,
	.right * {
		color: #444444;
		display: inline-block;
		margin-right: 22px;
	}
	.left *::after,
	.right *::after {
		content: "";
		position: absolute;
		margin-left: 12px;
		width: 1px;
		height: 16px;
		background-color: #444444;
	}
	.left *:last-child::after,
	.right *:last-child::after {
		background-color: transparent;
	}
	.left {
		float: left;
	}
	.right {
		float: right;
	}
</style>
