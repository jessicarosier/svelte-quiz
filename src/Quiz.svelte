<script>
    import { fade, blur, fly, slide  } from "svelte/transition";
    import { onMount, beforeUpdate, afterUpdate, onDestroy } from "svelte";
    import Question from "./Question.svelte";
    import Modal from "./Modal.svelte";
    import { score } from "./store.js";
    let activeQuestion = 0;
    let quiz = getQuiz();
    let isModalOpen = false;

    // onMount is a lifecycle hook that will run when the component is mounted to the DOM
    onMount(() => {
        console.log("mounted");
        console.log(score);
    });

    beforeUpdate(() => {
        console.log("before update");
    });

    afterUpdate(() => {
        console.log("after update");
    });

    async function getQuiz() {
        const res = await fetch("https://opentdb.com/api.php?amount=10&category=12&difficulty=easy&type=multiple");
        const quiz = await res.json();
        return quiz;
    }

    function handleClick() {
        quiz = getQuiz();
    }

    function nextQuestion() {
        activeQuestion = activeQuestion + 1;
    }

    function resetQuiz() {
        isModalOpen = false;
        score.set(0);
        activeQuestion = 0;
        quiz = getQuiz();
    }


    // $: makes the variable or expression reactive
    // checks to see if the score is greater than 1 and if it is, it will alert the user that they won and reset the quiz
    $: if($score > 1) {
        isModalOpen = true;
    }

    // reactive statement that will update the current question number
    $: currentQuestion = activeQuestion + 1;

    $: if(activeQuestion === 10) {
        alert(`You got ${score} out of 10 correct`);
        resetQuiz();
    }

</script>

<style>
  .fade-wrapper {
      position: absolute;
  }

  .container {
      min-height: 500px;
  }
</style>


<div>

<!-- directive modifiers including once, preventDefault, stopPropagation, passive, and capture are used to modify the behavior of the event listener-->
  <button on:click|once ={resetQuiz}>Start New Quiz</button>

  <h3>My Score: {$score}</h3>
  <h4>Question #{currentQuestion}</h4>

<div class="container">
  {#await quiz}
    Loading...
  {:then data}

    {#each data.results as question, index}
      {#if index === activeQuestion}
        <div in:fly={{x:100}} out:fly={{x:-200}} class="fade-wrapper">
        <Question {nextQuestion} {question}/>
        </div>
      {/if}
    {/each}

  {/await}

</div>

</div>

{#if isModalOpen}
<Modal on:close={resetQuiz}>
  <h2>You Won!</h2>
  <p>Congratulations!</p>
  <button on:click={resetQuiz}>Play Again</button>
</Modal>
{/if}


