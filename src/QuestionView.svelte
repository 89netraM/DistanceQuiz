<script lang="ts">
	export let options: ReadonlyArray<string>;
</script>

<div>
	{#each options as option}
		<img src={option} />
	{/each}
</div>

<style lang="scss">
	div {
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: center;
		align-content: center;

		> img {
			flex-grow: 1;
			flex-basis: 50%;
			max-width: 50%;
			max-height: 50%;
			object-fit: cover;
		}
	}
</style>
