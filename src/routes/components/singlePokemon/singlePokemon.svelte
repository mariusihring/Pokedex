<script lang="ts">
	import { onMount } from 'svelte';
	export let pokemon: any;
	let sprite: string;
	let name: string = pokemon.name;

	onMount(() => {
		sprite = pokemon.sprites.front_default;
		name = name.replace(/^\w/, (c) => c.toUpperCase());
	});
</script>

<section>
	{#if pokemon.name === undefined}
		<p />
	{:else}
		<div class="card lg:card-side bg-base-100 shadow-xl mt-5">
			<figure><img src={sprite} alt="Album" class="h-64 w-64" /></figure>
			<div class="card-body">
				<h2 class="card-title">{name}</h2>
				<p>Click the button to listen on Spotiwhy app.</p>
				<div class="card-actions justify-end">
					<button class="btn btn-primary">Listen</button>
				</div>
			</div>
		</div>
	{/if}
</section>
