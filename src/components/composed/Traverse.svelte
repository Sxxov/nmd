<script lang="ts">
	export let object: Record<any, any>;
</script>

{#each Object.entries(object) as [key, value]}
	{#if Object(value) !== value}
		<svelte:self obj={value} let:key let:value>
			<slot {key} {value} />
		</svelte:self>
	{:else}
		<slot {key} {value} />
	{/if}
{/each}
