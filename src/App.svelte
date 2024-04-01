<script lang="ts">
	import QuestionView from "./QuestionView.svelte";
</script>

<main>
	<QuestionView
		options={[
			"https://åsberg.net/me.jpg",
			"https://åsberg.net/denmark.jpg",
			"https://åsberg.net/me.jpg",
			"https://åsberg.net/denmark.jpg",
		]}
	/>
</main>

<style lang="scss">
	main {
		width: 100%;
		height: 100%;
	}
</style>
