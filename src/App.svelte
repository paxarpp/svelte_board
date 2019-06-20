<script>
	import Navbar from './Navbar.svelte';
	import Card from './Card.svelte';
	import AddPlayer from './AddPlayer.svelte';
	import { flip } from 'svelte/animate';
	import { quintOut } from 'svelte/easing';
	import { crossfade } from 'svelte/transition';
	import { onMount } from 'svelte';

	const [send, receive] = crossfade({
		duration: d => Math.sqrt(d * 200),

		fallback(node, params) {
			const style = getComputedStyle(node);
			const transform = style.transform === 'none' ? '' : style.transform;

			return {
				duration: 600,
				easing: quintOut,
				css: t => `
					transform: ${transform} scale(${t});
					opacity: ${t}
				`
			};
		}
	});

	let players = [];

	const addPlayer = (e) => {
		const newPlayer = {...e.detail, id: Math.max(...players.map(p => +p.id)) + 1};
		players = [...players, newPlayer];
	};
	const removePlayer = (e) => {
		const id = e.detail;
		players = players.filter( player => player.id !== id);
	};
	const updatePlayer = (e) => {
		const { id, points } = e.detail;
		players = players.map( player => {
			if (player.id === id) {
				player.points = points;
			}
			return player;
		});
	};
	onMount(() => {
		fetchUser();
	});
	const fetchUser = () => {
		setTimeout(() => {
			players = [
				{name: 'Jon', points: 50, id: 1},
				{name: 'Kris', points: 30, id: 2},
				{name: 'Rick', points: 72, id: 3},
			];
		}, 1000);
	};
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
		{#each players.sort((a, b) => a.points - b.points) as player (player.id)}
		<div
			in:receive="{{key: player.id}}"
			out:send="{{key: player.id}}"
			animate:flip
		>
			<Card {...player} on:removeplayer={removePlayer} on:updateplayer={updatePlayer} />
		</div>
		{/each}
	{/if}
</div>
