<script lang="ts">
	import CheerpJ from '$lib/CheerpJ.svelte';
	import Icon from '@iconify/svelte';
	import { isRunning } from './state';
	import Loading from '$lib/Loading.svelte';
	import { browser } from '$app/environment';

	let cjConsole: HTMLPreElement;
	export let isOutputMode = true;

	function viewInOutputMode() {
		if (browser) {
			const url = `/output/${window.location.hash}`;
			window.open(url, "_blank", "noreferrer");
		}
	}
</script>

<div class="w-full h-full" class:hidden={!$isRunning}>
	<Loading />
</div>

<div class="w-full h-full grid grid-cols-1 grow">
	<section class="border-r border-stone-200 dark:border-stone-700">
		<div class="p-3 h-full overflow-scroll text-stone-800 dark:text-stone-100">
			<div class="flex text-stone-500 text-sm select-none pb-3">
				Console

				<button
					class="ml-auto text-xs hover:underline text-stone-400 dark:text-stone-600"
					on:click={() => (cjConsole.innerText = '')}
				>
					Clear
				</button>
			</div>

			<!-- CheerpJ implicitly looks for a #console to write to -->
			<p class="font-mono text-sm h-0" bind:this={cjConsole} id="console"> </p>
		</div>
	</section>
</div>

<style>
	:global(#cheerpjDisplay) {
		box-shadow: none;
	}
</style>

<CheerpJ/>
