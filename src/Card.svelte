<script>
	import { createEventDispatcher } from 'svelte';
	import { slide } from 'svelte/transition';

	export let name;
	export let points;
	export let id;
  let showControls = false;
  
  const dispatcher = createEventDispatcher();

	const addPoint = () => {
		points += 1;
		dispatcher('updateplayer', { points, id });
	}
	const changePoint = (e) => {
		points = +e.target.value;
		dispatcher('updateplayer', { points, id });
	}
	const removePoint = () => {
		points -= 1;
		dispatcher('updateplayer', { points, id });
	}
	const toggleShow = () => showControls = !showControls;
	const onRemove = () => {
    dispatcher('removeplayer', id);
  };
</script>

<style>
	h3, h1 {
		margin: 5px 0;	
	}
	.card {
		background-color: beige;
		position: relative;
		padding: 10px;
		margin: 10px 0;
		box-sizing: border-box;
		border: 1px solid gray;
		max-width: 500px; 
	}
	.btn {
		cursor: pointer;
		padding: 5px;
		box-sizing: border-box;
		border: 1px solid gray;
		min-width: 3em;
	}
	.toggler {
		cursor: pointer;
		border: none;
		background-color: inherit;
	}
	.btn.danger{
		background-color: brown;
	}
	input {
    display: block;
  }
  .remove {
    background-color: inherit;
    border: none;
    color: red;
    position: absolute;
    top: 0.35rem;
    right: 0.5rem;
    cursor: pointer;
  }
</style>

<div class="card">
<button class="remove" on:click={onRemove}>X</button>
  <h1>
    {name}
    <button class="toggler" on:click={toggleShow}>
      {#if showControls}-{:else}+{/if}
    </button>
  </h1>
  <h3>Points: {points}</h3>
  {#if showControls}
	<div transition:slide>
		<button class="btn" on:click={addPoint}>+1</button>
		<button class="btn danger" on:click={removePoint}>-1</button>
		<input type="number" bind:value={points} on:change={changePoint}>
	</div>
  {/if}
</div>
