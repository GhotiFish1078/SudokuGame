<script setup>
import { ref } from 'vue';

const props = defineProps({
    cellNumber: {
        type: Number,
        required: true,
        default: 0,
    },
});

</script>

<template>
    <div class="non-editable-sudoku-cell">
        <svg class="chosen-number" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" width="100%" height="100%">
            <text x="50" y="50" font-family="Roboto, sans-serif" font-size="100" text-anchor="middle" dominant-baseline="central">
                {{ props.cellNumber }}
            </text>
        </svg>
    </div>
</template>

<style scoped>
    .non-editable-sudoku-cell {
        box-sizing: border-box;
        background-color: lightgray;

        /* Set this to 50% for mpw */
        aspect-ratio: 1 / 1;
        width: 100%;
    }

    .chosen-number {
        display: block;
    }

    text {
        user-select: none;
    }

</style>