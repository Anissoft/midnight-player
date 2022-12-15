<script lang="ts">
	import { page } from '$app/stores';

	export let videoUrl: string;
	export let videoTimestamp: number;
	export let desiredTime: Date;

	let isYouTube = videoUrl.indexOf('youtube.') !== -1;
	let player: HTMLIFrameElement | HTMLVideoElement;
	let parsedVideoUrl = videoUrl;
	let timer: ReturnType<typeof setTimeout>;
	let countdownTimer: ReturnType<typeof setInterval>;
	let countdown: string;

	$: {
		clearTimeout(timer);
		clearInterval(countdownTimer);
		let timeLeft: number = desiredTime.getTime() - Date.now() - videoTimestamp * 1000;

		if (videoUrl.indexOf('youtube.') !== -1) {
			const [, videoId] = videoUrl.match(/v=([^&]*)/) || [];

			parsedVideoUrl = `${$page.url.protocol}://www.youtube.com/embed/${videoId}?enablejsapi=1&origin=${$page.url.origin}`;
			isYouTube = true;

			// preload
			setTimeout(() => {
				(player as HTMLIFrameElement)?.contentWindow!.postMessage(
					JSON.stringify({ event: 'command', func: 'playVideo' }),
					'https://www.youtube.com'
				);
				(player as HTMLIFrameElement)?.contentWindow!.postMessage(
					JSON.stringify({ event: 'command', func: 'pauseVideo' }),
					'https://www.youtube.com'
				);
			}, 1000);

			timer = setTimeout(() => {
				countdown = '';
				clearInterval(countdownTimer);

				(player as HTMLIFrameElement)?.contentWindow!.postMessage(
					JSON.stringify({ event: 'command', func: 'playVideo' }),
					'https://www.youtube.com'
				);
			}, timeLeft);
		} else {
			isYouTube = false;
			parsedVideoUrl = videoUrl;

			timer = setTimeout(() => {
				countdown = '';
				clearInterval(countdownTimer);

				(player as HTMLVideoElement).play();
			}, timeLeft);
		}

		countdownTimer = setInterval(() => {
			countdown = new Date(desiredTime.getTime() - Date.now() - videoTimestamp * 1000)
				.toISOString()
				.substring(11, 19);
		}, 1000);
	}
</script>

<div class="root">
	{#if !videoUrl}
		<span class="message"><i>No video provided</i></span>
	{:else if isYouTube === true}
		<iframe
			bind:this={player}
			id="player"
			title="player"
			src={parsedVideoUrl}
			frameborder="0"
			allowfullscreen
		/>
	{:else}
		<!-- svelte-ignore a11y-media-has-caption -->
		<video bind:this={player} id="player" title="player" controls>
			<source src={parsedVideoUrl} type="video/mp4" />
		</video>
	{/if}
	{#if videoUrl && countdown}
		<div class="countdown">
			{countdown}
		</div>
	{/if}
</div>

<style>
	.root {
		position: relative;

		width: 100%;
		border: 1px dashed #999999;

		display: flex;
		flex-grow: 1;
		justify-content: center;
		align-items: center;
	}

	.root > #player {
		width: 100%;
		height: 100%;
	}

	.message {
		opacity: 0.5;
	}

	.countdown {
		position: absolute;
		top: 16px;

		color: red;
	}
</style>
