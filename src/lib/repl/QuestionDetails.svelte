<script lang="ts">

	import Loading from "$lib/Loading.svelte";
	import { onMount } from "svelte";
	import { decompress, type Fiddle, defaultFiddle, defaultFiddleComp } from '$lib/compress-fiddle.js';
	import { files, fiddleTitle, fiddleUpdated, questionsList } from '$lib/repl/state';
      async function fetchQuestionFile(path: string) {
            const res = await fetch(`/questions/${path}`);
            return res.text();
	}

    let examples: any[] = [];
	onMount(async () => {
		examples = $questionsList;
	});



		function setFiddle() {
		const fragmentURL: string = window.location.hash.slice(1);
		let fiddle: Fiddle;
		if (!fragmentURL) {
			fiddle = defaultFiddle;
			
		} else {
			fiddle = decompress(fragmentURL);
			
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

{#each examples as fiddle}
	{#if $fiddleTitle === fiddle.title}
  {@html fiddle.files[0].text}
{/if}
{:else}
	<div class="flex justify-center grow">
		<Loading />
	</div>
{/each}