<script>
	import '../app.postcss';
	import Header from './Header.svelte';
</script>

<div class="app">
	<header />

	<main>
		<slot />
	</main>

	<footer />
</div>

<style>
</style>
