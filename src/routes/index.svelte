<script lang="ts">
	import { onMount } from 'svelte';
	import PokemonCard from './components/pokemoncard/PokemonCard.svelte';
	import SinglePokemon from './components/singlePokemon/singlePokemon.svelte';
	let url: string = 'https://pokeapi.co/api/v2/pokemon?limit=1000?offset=0';
	let pokemon: any[] = [];
	let specificPoke: any[] = [];
	let Query: string = '';
	let singleSearch = false;
	function getAllPoke() {
		fetchPokeNames();
	}
	function getSpecificPoke() {
		fetchSpecificPoke();
	}
	async function fetchPokeNames() {
		await fetch(url)
			.then((response) => response.json())
			.then((data) => {
				pokemon = data.results;
			});
	}
	async function fetchSpecificPoke() {
		await fetch(`https://pokeapi.co/api/v2/pokemon/${Query.toLowerCase()}`)
			.then((response) => response.json())
			.then((data) => {
				specificPoke = data;
				console.log(specificPoke);
			});
	}
</script>

<svelte:head>
	<title>Pokedex</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<div class="grid align-center  px-4 py-4 w-full overflow-hidden">
		<input class="input input-bordered w-full text-center mb-4" bind:value={Query} />

		{#if Query === undefined}
			<button class="btn" on:click={getAllPoke}>Get All Pokemon</button>
		{:else}
			<button class="btn" on:click={getSpecificPoke}>Get Pokemon from Search</button>
		{/if}
		{#if pokemon.length >= 1}
			<div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
				{#each pokemon as poke}
					<PokemonCard pokemon={poke} />
				{/each}
			</div>
		{:else if Query != ''}
			<SinglePokemon pokemon={specificPoke} />
		{/if}
	</div>
</section>
