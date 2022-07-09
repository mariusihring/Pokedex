<script lang="ts">
	export let pokemon: any;
</script>

<section>
	{#if pokemon.name === undefined}
		<p />
	{:else}
		<p>{pokemon.name}</p>
	{/if}
</section>
