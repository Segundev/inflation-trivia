<script>
	import '../style.css';

	import HomeScreen from '../lib/HomeScreen.svelte';
	import GameScreen from '../lib/GameScreen.svelte';

	let state = 'home';
</script>

<div id="page">
	{#if state === 'home'}
		<HomeScreen on:clickNext={(e) => (state = e.detail)} />
	{:else if state === 'game'}
		<GameScreen on:restart={(e) => (state = e.detail)} />
	{/if}
</div>

<style>
	#page {
		height: 100vh;
		overflow: hidden;
	}
</style>
