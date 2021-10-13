<script>
    import { fade, blur, fly, slide, scale } from "svelte/transition";
    import { createEventDispatcher } from 'svelte';

    let w;
    const dispatch = createEventDispatcher();
</script>

<style>

</style>

<div class="absolute top-0 left-0 right-0 bottom-0 w-full h-full bg-gray-700 bg-opacity-60" transition:fade bind:clientWidth={w}>
    <div class="max-w-lg max-h-48 m-auto modal fixed top-0 left-0 right-0 bottom-0 bg-gray-100 p-5 border rounded-md" transition:fly={{ y: -100 }}>
        <button on:click={() => {
            dispatch('close');
        }} class="absolute top-2 right-2 border bg-gray-500 text-white px-3 py-1">Close</button>
        <slot name="modal">
        </slot>
    </div>
</div>
