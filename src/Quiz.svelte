<script>
    export let quizName;

    import { fade, blur, fly, slide, scale } from "svelte/transition";
    import { onMount, beforeUpdate, afterUpdate, onDestroy } from 'svelte';
    import Question from "./Question.svelte";
    import Modal from "./Modal.svelte";
    import { score } from './store.js';

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
    let quiz = getQuiz();
    let isModalOpen = false;

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
        isModalOpen = false;
        score.set(0);
        activeQuestion = 0;
        quiz = getQuiz();
    }

    // Reactive Statement
    $: if ($score > 0){
        isModalOpen = true;
    }

    // Reactive Declaration
    $: questionNumber = activeQuestion + 1;

</script>

<div class="py-5 text-center">
    <h1 class="text-4xl text-green-600 mb-5">{quizName}</h1>

    <button on:click={resetQuiz} class="px-5 py-1 mb-3 bg-purple-500 text-white">Start New Quiz</button>

    <h2 class="text-2xl text-green-600 mb-2">My score: {$score}</h2>
    <p class="mb-5">Question #{questionNumber}</p>

    {#await quiz}
        Loading ...
    {:then data}
        {#each data.results as question, index}
            {#if index == activeQuestion}
            <div class="absolute w-full max-w-3xl mx-auto p-5 border rounded-lg bg-gray-100" in:fly={{ x: -200}} out:fly={{ x: 200}}>
                <Question {nextQuestion} {question} />
            </div>
            {/if}
        {/each}
    {/await}
</div>

{#if isModalOpen}
    <Modal on:close={resetQuiz}>
        <div slot="modal" class="flex flex-col justify-center items-center mt-3">
            <h2 class="text-lg mb-3">You won!</h2>
            <p class="mb-3">Congratulations</p>
            <button on:click={resetQuiz} class="border bg-green-300 px-3 py-1">Start Over</button>
        </div>
    </Modal>
{/if}