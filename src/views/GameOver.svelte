<script>
	import { createEventDispatcher } from "svelte"
  export let gameStats
	const dispatch = createEventDispatcher()

  const restart = () => {
    dispatch('restart')
  }

	const startPage = () => {
    dispatch('startPage')
  }
</script>

<main>
  <h1>Game Over</h1>
  <h3>Score: {gameStats.score}</h3>
  <h3>Lives: {gameStats.lives}</h3>
  <h3>Questions Answerd: {gameStats.questionNumber}</h3>
  <button on:click={restart}>Try Again!</button>
  <button on:click={startPage}>Back to Start</button>
</main>

<style>
  	main {
    position: relative;
		text-align: center;
		padding: 1em;
		width: 800px;
		margin: 0 auto;
	}

  h1 {
		color: #ff3e00;
		text-transform: uppercase;
		margin-top:0;
		font-size: 4em;
		font-weight: 100;
	}

  h3 {
		color: #ff3e00;
		text-transform: uppercase;
		margin-top:0;
		font-size: 1.5em;
		font-weight: 100;
    padding-right: 20px;
	}

	button {
		width: 150px;
		margin: 0 10px;
	}
</style>
