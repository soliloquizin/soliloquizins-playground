<script>
	import Video from './components/Video.svelte';
</script>

<main>
	<h1>playground</h1>
	<Video />
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>