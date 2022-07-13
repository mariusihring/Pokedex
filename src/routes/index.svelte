<script lang="ts">
	import PokemonCard from './components/pokemoncard/PokemonCard.svelte';
	import SinglePokemon from './components/singlePokemon/singlePokemon.svelte';
	let url: string = 'https://pokeapi.co/api/v2/pokemon?limit=1000?offset=0';
	let pokemon: any[] = [];
	let Query: string = '';
	let loading: Promise<any> | undefined;

	function fetchData() {
		if (Query != '') {
			loading = fetchSpecificPoke();
			Query = '';
		} else {
			loading = fetchAllPoke();
		}
	}

	async function fetchAllPoke() {
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
				pokemon = data;
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

		<button class="btn" on:click={fetchData}
			>{Query == '' ? 'Get all Pokemon' : 'Get Pokemon from Search'}</button
		>
		{#await loading}
			<button type="button" class="bg-indigo-500 ..." disabled>
				<svg class="animate-spin h-5 w-5 mr-3 ..." viewBox="0 0 24 24">
					<!-- ... -->
				</svg>
				Loading...
			</button>
		{:then number}
			{#if pokemon.length >= 2}
				<div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-2 lg:grid-cols-3 gap-4">
					{#each pokemon as poke}
						<PokemonCard pokemon={poke} />
					{/each}
				</div>
			{:else if (pokemon.length = 1)}
				<SinglePokemon {pokemon} />
			{/if}
		{:catch error}
			<p style="color: red">Could not load</p>
		{/await}
	</div>
</section>
