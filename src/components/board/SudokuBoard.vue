<script setup>
import { reactive, ref } from 'vue';
import NineBlock from './NineBlock.vue';

let puzzleBoard = [];
let solutionBoard = [];
let difficulty = "";

// Double array
const playerBoard = ref([]);
const nineBlocks = ref([]);

// -1: No selected cell initially
const selectedCell = reactive({parent: -1, child: -1});

const isLoading = ref(true);

const createNineBlocks = () => {
    nineBlocks.value = [];

    for (let row = 0; row < 3; row++) {
        for (let col = 0; col < 3; col++) {
            let nineBlock = [];

            for (let i = row * 3; i < (row + 1) * 3; i++) {
                for (let j = col * 3; j < (col + 1) * 3; j++) {
                    nineBlock.push(puzzleBoard[i][j]);
                }
            }

            nineBlocks.value.push(nineBlock);
        }
    }
};

const fetchPuzzle = async () => {
    try {
        const response = await fetch("https://sudoku-api.vercel.app/api/dosuku");
        if (!response.ok) {
            throw new Error(`Response status: ${response.status}`);
        }

        const data = await response.json();
        const grid = data.newboard.grids[0];
        puzzleBoard = grid.value;
        solutionBoard = grid.solution;
        difficulty = grid.difficulty;

        playerBoard.value = JSON.parse(JSON.stringify(grid.value));

        // Create the nineBlocks
        createNineBlocks();

        console.log("Puzzle Board:", puzzleBoard);
        console.log("Solution Board:", solutionBoard);
        console.log("Difficulty:", difficulty);
    } catch (e) {
        console.log("An error occurred" + e.message);
    } finally {
        isLoading.value = false;
    }
};

const updateChosenCell = (e) => {
    selectedCell.parent = e.parent;
    selectedCell.child = e.child;
};

const updatePlayerBoard = (e) => {
    const number = e.number === '' ? 0 : parseInt(e.number, 10);
    console.log(e.row);
    console.log(e.col);
    playerBoard.value[e.row][e.col] = number;
    console.log(playerBoard.value);
};

fetchPuzzle();

</script>

<template>
    <div v-if="isLoading">
    </div>
    <div v-else class="sudoku-board-two">
        <template v-for="(i, index) in 9">
            <NineBlock v-bind="{ values: nineBlocks[index], 'selected-cell': selectedCell, 'parent-index': index }" v-on:re-emit-event="updateChosenCell" v-on:re-emit-number="updatePlayerBoard"/>
        </template>
    </div>
</template>

<style scoped>
.sudoku-board-two {
    display: grid;
    grid-template-rows: repeat(3, 1fr);
    grid-template-columns: repeat(3, 1fr);

    aspect-ratio: 1 / 1;
    width: 50%;

    border-color: black;
    border-width: 3px;
    border-style: solid;

    gap: 3px;
    background-color: black;
}

</style>