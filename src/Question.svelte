<script>
    export let question;
    export let nextQuestion;
    export let addToScore;

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
            addToScore();
        }
    }
</script>

<h3 class="text-2xl mb-3">{@html question.question}</h3>

{#if isAnswered}
    <h3 class="text-md mb-3">
        {#if isCorrect}
            <span class="text-green-600">You got it right</span>
        {:else}
            <span class="text-red-600">You goofed up</span>
        {/if}
    </h3>
{/if}

{#if isAnswered}
    <div class="flex space-x-2 mb-3">
        <button on:click={() => nextQuestion()} class="border bg-green-300 px-3 py-1">Next question</button>
    </div>
{:else}
    <div class="flex space-x-2 mb-3">
        {#each allAnswers as answer}
            <button on:click={() => checkQuestion(answer.correct)} class="border bg-gray-200 px-3 py-1">
                {@html answer.answer}
            </button>
        {/each}
    </div>
{/if}
