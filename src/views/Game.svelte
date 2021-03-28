<script>
  import { createEventDispatcher } from "svelte"
	const dispatch = createEventDispatcher()
  let quizData = []
  let question = {}
  let questionNumber = 1
  let score = 0
  let lives = 3
  let time = 15
  let timerInterval

  let isTimeLifelineUsed = false
  let isEliminateQuestionLifeLineUsed = false

  fetch('https://opentdb.com/api.php?amount=10&category=21&difficulty=medium&type=multiple')
  .then((response) => response.json()) 
  .then((json) => {
    quizData = json.results
    getQuestion(questionNumber)
    timer()
  })

  const getQuestion = (number, eliminateQuestion) => {
    question = {
      question: quizData[number].question,
      rightAnswer: quizData[number].correct_answer,
      answers: [...quizData[number].incorrect_answers]
    }

    if(eliminateQuestion) question.answers.splice(1)
    question.answers.splice(Math.floor(Math.random() * 4), 0, question.rightAnswer)
  }

  const submitAnswer = (e) => {
    if(lives <= 0 || questionNumber >= 10) {
      clearInterval(timerInterval)
      dispatch('gameOver', {
        lives,
        score,
        questionNumber
      })
      return
    }

    if(e.target.innerHTML === question.rightAnswer) {
      score++
    } else {
      lives--
    }
    questionNumber++
    getQuestion(questionNumber)
    time = 15
  }

  const timer = () => {
    timerInterval = setInterval(()=> {
        time--
        if(time < 0 && lives <= 0) {
          clearInterval(timerInterval)
          dispatch('gameOver', {
            lives,
            score,
            questionNumber
          })
        }
        if(time < 0){
          time = 15
          lives--
          questionNumber++
          getQuestion(questionNumber)
        }
      }, 1000)
  }

  const timeLifeline = () => {
    if (isTimeLifelineUsed) return 
    time += 10
    isTimeLifelineUsed = true
  }

  const eliminateLifeline = () => {
    if (isEliminateQuestionLifeLineUsed) return
    isEliminateQuestionLifeLineUsed = true
    getQuestion(questionNumber, true)
  }
</script>

<main>
  <header>
    <div class="lifelines">
      <h3 class:disabled={isTimeLifelineUsed} on:click={timeLifeline}>+10 sec</h3>
      <h3 class:disabled={isEliminateQuestionLifeLineUsed} on:click={eliminateLifeline}>50/50</h3>
    </div>
    <div>
      <h3>Time: {time}</h3>
      <h3>Lives: {lives}</h3>
      <h3>Score: {score}</h3>
    </div>
  </header>
  {#if quizData.length}
  <h1>Question {questionNumber}</h1>
  <h2>{question.question}</h2>
  {#each question.answers as answer}
  <div class="button" on:click={submitAnswer}>
    <h2>{answer}</h2>
  </div>
  {/each}
  {/if}
</main>

<style>
	main {
    position: relative;
		text-align: center;
		padding: 1em;
		width: 800px;
		margin: 0 auto;
	}

  header {
    position: fixed;
    display: flex;
    justify-content: space-between;
    top: 0;
    right: 0;
    left: 0;
    padding: 20px;
  }

  header div {
    display: flex;
  }

  .lifelines h3 {
    cursor: pointer;
    border-radius: 5px;
    border: 1px solid #ff3e00;
  }

  .lifelines h3:active {
    background-color: #f4f4f4;
  }

  .lifelines .disabled {
    border-color:#7a7a7a;
    color: #7a7a7a;
    cursor: default;
  }

  .lifelines .disabled:active {
    background-color: white;

  }

  h1 {
		color: #ff3e00;
		text-transform: uppercase;
		margin-top:0;
		font-size: 4em;
		font-weight: 100;
	}

  h2 {
		color: #ff3e00;
		text-transform: uppercase;
		margin-top:0;
		font-size: 1.5em;
		font-weight: 100;
	}

  h3 {
		color: #ff3e00;
		text-transform: uppercase;
		margin-top:0;
		font-size: 1.5em;
		font-weight: 100;
    margin-right: 20px;
    padding: 0 10px;
	}

  .button {
    padding: 10px;
    background-color: black;
    border-radius: 5px;
    margin: 20px;
    cursor: pointer;
  }

  .button:active {
    background-color: #2c2c2c;
  }

  .button h2 {
    margin-bottom: 0;
    color: white;
  }
</style>
