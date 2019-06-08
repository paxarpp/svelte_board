<script>
	import Navbar from './Navbar.svelte';
	import Card from './Card.svelte';
	import AddPlayer from './AddPlayer.svelte';

	let players = [
		{name: 'Jon', points: 53, id: 1},
		{name: 'Kris', points: 27, id: 2},
		{name: 'Rick', points: 68, id: 3},
	];
	const addPlayer = (e) => {
		const newPlayer = e.detail;
		players = [...players, newPlayer];
	}
	const removePlayer = (e) => {
		const name = e.detail;
		players = players.filter( player => player.name !== name);
	}
</script>

<style>
	.container {
		display: flex;
		flex-flow: column nowrap;
		width: 100%;
		height: 100%;
	}
</style>

<Navbar />
<div class="container">
	<AddPlayer on:addplayer={addPlayer} />
	{#if players.length === 0}
		<p>No players</p>
	{:else}
		{#each players as player}
			<Card {...player} on:removeplayer={removePlayer} />
		{/each}
	{/if}
</div>
