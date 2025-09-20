<script lang="ts">
	import '../../app.css';
	import { effectiveTheme } from '$lib/settings/store';
	import { files, fiddleTitle, fiddleUpdated } from '$lib/repl/state';
	import { decompress, type Fiddle, defaultFiddle, defaultFiddleComp } from '$lib/compress-fiddle.js';
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';

	let description = 'StreamCode is fork of javafiddle powered by cheerpj';

	function setFiddle() {
		const fragmentURL: string = window.location.hash.slice(1);
		let fiddle: Fiddle;
		if (!fragmentURL) {
			fiddle = defaultFiddle;
			goto(`/#${defaultFiddleComp}`);
		} else {
			fiddle = decompress(fragmentURL);
			description = fiddle.files[0].content;
		}
		$files = fiddle.files;
		$fiddleTitle = fiddle.title;
		$fiddleUpdated = fiddle.updated;
	}

	onMount(() => {
		window.addEventListener('popstate', setFiddle);
		setFiddle();
		return () => {
			window.removeEventListener('popstate', setFiddle);
		};
	});
</script>

<svelte:head>
	<title
		>{$fiddleTitle
			? `${$fiddleTitle} - StreamCode`
			: 'StreamCode'}</title
	>
	<meta name="description" content={description} />
</svelte:head>

<div class="contents" class:dark={$effectiveTheme === 'dark'}>
	<div class="bg-white text-black dark:bg-stone-900 dark:text-white">
		<slot />
	</div>
</div>
