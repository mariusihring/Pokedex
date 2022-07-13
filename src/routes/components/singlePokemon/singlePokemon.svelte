<script lang="ts">
	import { onMount } from 'svelte';
	export let pokemon: any;
	let pokemonSpeciesUrl: string;
	let name: string = pokemon.name;

	let evolutionChain: any = [];
	async function fetchSpecies() {
		pokemonSpeciesUrl = pokemon.species.url;
		await fetch(pokemonSpeciesUrl)
			.then((response) => response.json())
			.then((data) => {
				fetch(data.evolution_chain.url)
					.then((response) => response.json())
					.then((data) => {
						saveEvolution(data.chain);
					});
			});
	}

	onMount(() => {
		fetchSpecies();
	});

	function saveEvolution(data: any) {
		for (let i = 0; i < 3; i++) {
			evolutionChain.push(data.species.name);
			data = data.evolves_to[0];
			console.log(evolutionChain);
		}
	}
</script>

<section>
	{#if pokemon.name === undefined}
		<p />
	{:else}
		<div class="card lg:card-side bg-base-100 shadow-xl mt-5">
			<figure><img src={pokemon?.sprites?.front_default} alt="Album" class="h-64 w-64" /></figure>
			<div class="card-body">
				<h2 class="card-title">{(name = name.replace(/^\w/, (c) => c.toUpperCase()))}</h2>
			</div>
		</div>
	{/if}
</section>
