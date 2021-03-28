<script>
	import { createEventDispatcher } from "svelte";
	const dispatch = createEventDispatcher();

	const startGame = () => {
		dispatch("start")
	}
</script>

<main>
	<h1>Svelte Quiz App!</h1>
	<p>This is a 10 question quiz about sports, you will have 15 seconds on every question to select an answer.
	You will have 2 lifelines one "50/50" where two of the wrong answers will disappear, and one "+10 sec" where you will get another 10 seconds to answer the current question.
	After the quiz is finished your score will be displayed on the screen.
	Good Luck!</p >
	<button on:click={startGame}>Start Game</button>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		width: 800px;
		margin: 0 auto;
	}

	P {
		margin-bottom: 50px;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		margin-top:0;
		font-size: 4em;
		font-weight: 100;
	}

	button {
		width: 150px;
	}
</style>
