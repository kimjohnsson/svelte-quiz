<script>
	import StartPage from './views/StartPage.svelte'
	import Game from './views/Game.svelte'
	import GameOver from './views/GameOver.svelte'

	let isGameStarted = false
	let isGameOver = false
	let gameStats = {}

	const startGame = () => {
		isGameStarted = true
	}

	const gameOver = (e) => {
		gameStats = e.detail
		isGameOver = true
	}

	const startPage = () => {
		isGameStarted = false
		isGameOver = false
		gameStats = {}
	}

	const restart  = () => {
		gameStats = {}
		isGameOver = false
		isGameStarted = true
	}
</script>

<body>
	{#if !isGameStarted}
	<StartPage on:start={startGame}/>
	{:else if !isGameOver}
	<Game on:gameOver={gameOver}/>
	{:else}
	<GameOver gameStats={gameStats} on:restart={restart} on:startPage={startPage}/>
	{/if}
</body>

<style>
	body {
		display: flex;
		flex-direction: column;
		justify-content: center;
	}
</style>
