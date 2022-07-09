<script lang="ts">
	import { onMount } from 'svelte';
	import PokemonCard from './components/pokemoncard/PokemonCard.svelte';
	let url: string = 'https://pokeapi.co/api/v2/pokemon?limit=1000?offset=0';
	let pokemon: any[] = [];

	function getPoke() {
		fetchPokeNames();
	}
	async function fetchPokeNames() {
		await fetch(url)
			.then((response) => response.json())
			.then((data) => {
				pokemon = data.results;
			});
	}
</script>

<svelte:head>
	<title>Pokedex</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<div class="grid align-center  mx-4 my-4">
		<button class="btn" on:click={getPoke}>Fetch Pokemon</button>

		<div class="grid gap-7 grid-cols-4 mx-7 my-7">
			{#each pokemon as pokemon}
				<PokemonCard {pokemon} />
			{/each}
		</div>
	</div>
</section>
