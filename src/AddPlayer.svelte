<script>
  import { createEventDispatcher } from 'svelte';

  const dispatcher = createEventDispatcher();

  let player = {
    name: '',
    points: 0,
  };

  const onSubmit = (e) => {
    e.preventDefault();
    dispatcher('addplayer', player);
    player = {
      name: '',
      points: 0,
    };
  };
</script>

<style>
  form {
    padding: 10px;
    width: 500px;
    display: flex;
    flex-flow: column;
  }
</style>

<form on:submit={onSubmit}>
  <input type="text" placeholder="Player name" bind:value={player.name}>
  <input type="number" placeholder="Player points" bind:value={player.points}>
  <input type="submit" value="Add Player">
</form>