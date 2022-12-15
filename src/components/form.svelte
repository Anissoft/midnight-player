<script lang="ts">
	import { page } from '$app/stores';
	import { goto } from '$app/navigation';
	import { browser } from '$app/environment';
	import { onMount } from 'svelte';
	export let onChange: (values: { second: number; time: Date; source: string }) => void;

	let source = '';
	let second = 0;
	let time = '23:59:59';
  let mounted = false;

	const updateUrl = (event: { currentTarget: EventTarget & HTMLInputElement }) => {
		if (browser && event.currentTarget !== document.activeElement) {
			goto(`?${$page.url.searchParams.toString()}`);
		}
	};

	onMount(() => {
    mounted = true;
    source = $page.url.searchParams.get('source') || '';
    second = parseInt($page.url.searchParams.get('second') || '0');
    time = $page.url.searchParams.get('time') || '23:59:59';
	});

	$: {
    if (time === '') {
			time = '23:59:59';
		}

		let tagetTime = new Date(new Date().toString().split(':')[0].slice(0, -2) + time);

		if (tagetTime < new Date()) {
			tagetTime.setDate(tagetTime.getDate() + 1);
		}

		if (mounted) {
			$page.url.searchParams.set('source', source);
			$page.url.searchParams.set('second', second.toString());
			$page.url.searchParams.set('time', time);
		}

		onChange({
			second,
			source,
			time: tagetTime
		});
	}
</script>

<form class="root">
	<p class="container">
		<label for="source">Video from URL:</label>
		<input
			type="url"
			id="source"
			name="source"
			bind:value={source}
			on:blur={updateUrl}
			placeholder="YouTube link or direct link to the video file"
		/>
		<br />
		<label for="second">Should play the exact second:</label>
		<input
			type="number"
			id="second"
			name="second"
			min="0"
			max=""
			step="1"
			bind:value={second}
			on:change={updateUrl}
			on:input={updateUrl}
		/>
		<br />
		<label for="time">When current time is: </label>
		<input type="time" id="time" name="time" step="10" bind:value={time} on:blur={updateUrl} />
		<br />
	</p>
	<p />
</form>

<style>
	input {
		background-color: unset;
		border: none;
		font-size: 18px;
		border: 1px var(--border-color) solid;
		outline: none;
		color: var(--font-color);
	}

	label {
		display: inline-block;
		min-width: 250px;
	}

	.container {
		white-space: nowrap;
		line-height: 32px;

		font-size: 18px;
	}

	input#source {
		width: calc(100% - 250px);
	}

	input#second {
		width: 50px;
	}

	input#time {
		width: 200px;
	}

	@media (max-width: 768px) {
		.container {
			white-space: normal;
		}

		label,
		input#source {
			width: 100%;
			max-width: 100%;
		}

		input {
			margin-bottom: 16px;
		}
	}
</style>
