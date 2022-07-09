<script lang="ts">
	import { onMount } from 'svelte';
	export let pokemon: any;
	let pokeDetails: any;
	let sprite: string;
	let firstType: string;
	let secondType: string;
	let species: string;

	onMount(async () => {
		await getPokeDetails();
		sprite = pokeDetails.sprites.front_default;
		firstType = pokeDetails.types[0].type.name;
		secondType = pokeDetails.types[1].type.name;
		species = pokeDetails.species.url;
		await getPokeSpecies();
	});
	async function getPokeDetails() {
		await fetch(pokemon.url)
			.then((response) => response.json())
			.then((data) => {
				pokeDetails = data;
			});
	}
	async function getPokeSpecies() {
		await fetch(species)
			.then((response) => response.json())
			.then((data) => {});
	}
</script>

<section>
	<div class="align-center">
		<div class="card w-96 bg-slate-500 shadow-xl">
			<figure><img src={sprite} alt="Shoes" class="h-28 w-28" /></figure>
			<div class="card-body">
				<h2 class="card-title">
					{pokemon.name}
				</h2>
				<p>If a dog chews shoes whose shoes does he choose?</p>
				<div class="card-actions justify-end">
					<div class="badge badge-outline">{firstType}</div>
					{#if secondType != undefined}
						<div class="badge badge-outline">{secondType}</div>
					{/if}
				</div>
			</div>
		</div>
	</div>
</section>
