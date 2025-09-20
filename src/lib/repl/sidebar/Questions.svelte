<script lang="ts">
	import { goto } from '$app/navigation';
	import Loading from '$lib/Loading.svelte';
	import { compress, type Fiddle } from '$lib/compress-fiddle';
	import { onMount } from 'svelte';
	import { files, fiddleTitle, fiddleUpdated, favouriteIndex, questionsList } from '$lib/repl/state';

	async function fetchQuestionFile(path: string) {
		const res = await fetch(`/questions/${path}`);
		return res.text();
	}

	function openQuestion(example: Fiddle) {
		if ($favouriteIndex !== -1) {
			$favouriteIndex = -1;
		}
		$files = example.files;
		$fiddleTitle = example.title;
		$fiddleUpdated = example.updated;
		// update URL with fragment
		const fiddleFragmentURL = compress(example);
		goto(`/#${fiddleFragmentURL}`);
	}

	let examples: any[] = [];
	onMount(async () => {
		examples = $questionsList;
	});
</script>

{#each examples as fiddle}
	<button
		class="w-full text-left flex items-center px-4 py-2 hover:bg-stone-200 dark:hover:bg-stone-900"
		on:click={() => openQuestion(fiddle)}
	>
		<div class="grow">{fiddle.title}</div>
	</button>
{:else}
	<div class="flex justify-center grow">
		<Loading />
	</div>
{/each}
