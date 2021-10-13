<script>
    import { score } from "./store.js";

    export let question;
    export let nextQuestion;

    let isCorrect;
    let isAnswered = false;

    let answers = question.incorrect_answers.map(answer => {
        return {
            answer,
            correct: false
        }
    });
    let allAnswers = [
        ...answers,
        {
            answer: question.correct_answer,
            correct: true
        }
    ];
    shuffle(allAnswers);

    function shuffle(array) {
        array.sort(() => Math.random() - 0.5)
    }

    function checkQuestion(correct) {
        isAnswered = true;
        isCorrect = correct;
        if(correct) {
            score.update(val => val + 1);
        }
    }
</script>

<div class="text-center">

    <h3 class="text-2xl mb-5">{@html question.question}</h3>

    {#if isAnswered}
        <h3 class="text-md mb-3" class:text-green-600={isCorrect} class:text-red-600={!isCorrect}>
            {#if isCorrect}
                You got it right
            {:else}
                You goofed up
            {/if}
        </h3>
    {/if}

    <div class="flex justify-center space-x-2 mb-3">
        {#each allAnswers as answer}
            <button disabled={isAnswered} on:click={() => checkQuestion(answer.correct)} class="border bg-gray-200 px-3 py-1">
                {@html answer.answer}
            </button>
        {/each}
    </div>

    {#if isAnswered}
        <div class="flex justify-center space-x-2 mb-3">
            <button on:click={() => nextQuestion()} class="border bg-green-300 px-3 py-1">Next question</button>
        </div>
    {/if}

</div>