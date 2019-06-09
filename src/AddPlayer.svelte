<script>
  import { createEventDispatcher } from 'svelte';

  const dispatcher = createEventDispatcher();

  let error = false;
  let player = {
    name: '',
    points: 0,
  };

  const onSubmit = (e) => {
    e.preventDefault();
    if (!player.name.trim()) {
      error = true;
      return;
    }
    error = false;
    dispatcher('addplayer', player);
    player = {
      name: '',
      points: 0,
    };
  };
  const resetError = () => error = false;
</script>

<style>
  form {
    position: relative;
    padding: 10px;
    width: 500px;
    display: flex;
    flex-flow: column;
  }
  input[type="submit"] {
    background-color: lightblue;
    cursor: pointer;
  }
  .error {
    color: red;
    position: absolute;
    top: 2rem;
    left: 20px;
    font-size: 0.7rem;
  }
</style>

<form on:submit={onSubmit}>
{#if error}
  <label class="error">Имя не может быть пустым</label>
{/if}
  <input type="text" placeholder="Player name" bind:value={player.name} on:focus={resetError}>
  <input type="number" placeholder="Player points" bind:value={player.points}>
  <input type="submit" value="Add Player">
</form>