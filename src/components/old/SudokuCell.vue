<script setup>
import { ref, reactive, watch } from 'vue';

const chosenNumber = ref("");
const candidateNumbers = reactive({
    "1": false,
    "2": false,
    "3": false,
    "4": false,
    "5": false,
    "6": false,
    "7": false,
    "8": false,
    "9": false,
});

const isSelected = ref(false);

const handleClick = (e) => {
    if (!isSelected.value) {
        isSelected.value = !isSelected.value;
    }
}

const handleKeyBoardPress = (e) => {
    console.log("This is working");
    if (/[1-9]/.test(e.key)) {
        chosenNumber.value = e.key;
    }

    if (e.key === "Backspace" || e.key === "Delete") {
        chosenNumber.value = "";
    }
}

</script>

<template>
    <div class="sudoku-cell" v-bind:class="{ selected: isSelected }">

        <div class="candidate-number-box" tabindex="0" v-for="number in 9" v-bind:key="number" v-on:keydown="handleKeyBoardPress">
            <svg class="candidate-number" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" width="100%" height="100%">
                <text x="50" y="50" font-family="Roboto, sans-serif" font-size="100" text-anchor="middle" dominant-baseline="central">
                    {{ number }}
                </text>
            </svg>
        </div>

        <div class="chosen-number-box" tabindex="0" v-bind:class="{ selected: isSelected }" v-on:click="handleClick" v-on:keydown="handleKeyBoardPress">
            <svg class="chosen-number" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" width="100%" height="100%">
                <text x="50" y="50" font-family="Roboto, sans-serif" font-size="100" text-anchor="middle" dominant-baseline="central">
                    {{ chosenNumber }}
                </text>
            </svg>
        </div>
    </div>


</template>

<style scoped>
.sudoku-cell {
    display: grid;
    grid-template-rows: repeat(3, 1fr);
    grid-template-columns: repeat(3, 1fr);
    border: 1px solid black;

    aspect-ratio: 1 / 1;
    width: 50%;
    cursor: pointer;
}

/* Styling so that only the number itself lights up when selected */
.candidate-number-box {
    display: flex;
    justify-content: center;
    align-items: center;
    opacity: 0%;
    transition: opacity 0.2s linear;
}

.candidate-number-box:hover {
    opacity: 100%;
}

.chosen-number-box {
    display: inline-block;
    vertical-align: top;
    border-style: solid;
    border-color: black;
    width: 25%;
}

.selected {
    background-color: #42B883;
}

.chosen-number {
    display: block;
}
</style>