<script>
    export let quizName;

    import { fade, blur, fly, slide, scale } from "svelte/transition";
    import { onMount, beforeUpdate, afterUpdate, onDestroy } from 'svelte';
    import Question from "./Question.svelte";

    onMount(() => {
        console.log("I mounted");
    });
    beforeUpdate(() => {
        console.log("Before update");
    });
    afterUpdate(() => {
        console.log("After update");
    });
    onDestroy(() => {
        console.log("Destroy");
    });

    let activeQuestion = 0;
    let score = 0;
    let quiz = getQuiz();

    async function getQuiz() {
        const res = await fetch(
            "https://opentdb.com/api.php?amount=20&category=21&type=multiple"
        );
        const quiz = await res.json();
        return quiz;
    }

    function nextQuestion() {
        activeQuestion = activeQuestion + 1;
    }

    function resetQuiz() {
        score = 0;
        activeQuestion = 0;
        quiz = getQuiz();
    }

    function addToScore() {
        score = score + 1;
    }

    // Reactive Statement
    $: if (score > 4){
        alert('You won!');
        resetQuiz();
    }

    // Reactive Declaration
    $: questionNumber = activeQuestion + 1;

</script>

<div class="py-3">
    <h1 class="text-4xl text-green-600 mb-3">{quizName}</h1>

    <button on:click={resetQuiz} class="px-5 py-1 mb-3 bg-purple-500 text-white">Start New Quiz</button>

    <h2 class="text-2xl text-green-600 mb-2">My score: {score}</h2>
    <p class="mb-3">Question #{questionNumber}</p>

    {#await quiz}
        Loading ...
    {:then data}
        {#each data.results as question, index}
            {#if index == activeQuestion}
            <div class="absolute max-w-3xl" in:fly={{ x: -200}} out:fly={{ x: 200}}>
                <Question {addToScore} {nextQuestion} {question} />
            </div>
            {/if}
        {/each}
    {/await}
</div>