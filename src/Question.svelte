<script>
    import { score } from './store.js';
    export let question;
    export let nextQuestion;

    // export let addToScore;


    let isCorrect;
    let isAnswered = false;
    let answers = question.incorrect_answers.map(answer => {
        return {
            answer,
            correct: false
        };
    });

    let allAnswers = [...answers,
        {
            answer: question.correct_answer,
            correct: true
        }
    ];

    shuffle(allAnswers);

    function shuffle(array) {
        array.sort(() => Math.random() - 0.5);
        return array;
    }

    function checkQuestion(correct) {
        isAnswered = true;
        isCorrect = correct;
        if(correct) {
            score.update((val) => val += 1)
        }
    }

</script>

<style>
    :global(button) {
        border: 1px solid var(--secondary);
        background: var(--primary);
        color: var(--secondary);
        border-radius: 5px;
        display: block;

        &:hover {
            border: 1px solid var(--primary);
            background: var(--secondary);
            color: var(--primary);
            cursor: pointer;
        }
    }

    h5.isCorrect {
        color: green;
    }

    h5.wrong {
        color: red;
    }
</style>

<h2>
  {@html question.question}
</h2>

{#if isAnswered}
  <!-- if the answer is correct apply the correct class otherwise apply the wrong class-->
  <h5 class:isCorrect class:wrong={!isCorrect}>
    {#if isCorrect}
      You got it right!
    {:else}
      You got it wrong!
      The correct answer was: {@html question.correct_answer}
    {/if}
  </h5>
{/if}


{#each allAnswers as answer}
  <button disabled={isAnswered} on:click={ () => checkQuestion(answer.correct)}>
    {@html answer.answer}
  </button>
{/each}


{#if isAnswered}
  <button on:click={nextQuestion}>Next Question</button>
{/if}


