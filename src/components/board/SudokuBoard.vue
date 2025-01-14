<script setup>
import { reactive, ref } from 'vue';
import NineBlock from './NineBlock.vue';
import SelectionArea from '../misc/SelectionArea.vue';
import SubmissionSpace from '../misc/SubmissionSpace.vue';

let puzzleBoard = [];
let solutionBoard = [];
let difficulty = "";

// Double array
const playerBoard = ref([]);
const nineBlocks = ref([]);

// -1: No selected cell initially
const selectedCell = reactive({ parent: -1, child: -1 });

// normal by default
const selectionMode = ref("normal");
const numberSelection = ref(''); 

const isLoading = ref(true);

const isSolved = ref(false);

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
    isLoading.value = true;
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
    } catch (e) {
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
    playerBoard.value[e.row][e.col] = number;
};

const handleSelectionChoice = (selection) => {
    numberSelection.value = selection;
};

const handleSelectionMode = (mode) => {
    selectionMode.value = mode;
};

const checkBoard = () => {
    for (let i = 0; i < 9; i++) {
        for (let j = 0; j < 9; j++) {
            if (playerBoard.value[i][j] !== solutionBoard[i][j]) {
                isSolved.value = false;
                return;
            }
        }
    }

    isSolved.value = true;
};

const resetBoard = () => {
    playerBoard.value = JSON.parse(JSON.stringify(puzzleBoard));
    createNineBlocks();
};

fetchPuzzle();

</script>

<template>
    <div v-if="isLoading">
    </div>
    
    <div v-else-if="!isLoading && !isSolved" class="play-area">
        <div class="sudoku-board-two">
            <template v-for="(i, index) in 9">
                <NineBlock v-bind="{ values: nineBlocks[index], 'selected-cell': selectedCell, 'parent-index': index, 'selection-mode': selectionMode, 'number-selection': numberSelection }" v-on:re-emit-event="updateChosenCell" v-on:re-emit-number="updatePlayerBoard"/>
            </template>
        </div>
        <div class="submission-space">
            <SubmissionSpace v-on:submit="checkBoard()" v-on:new-puzzle="fetchPuzzle()"/>
        </div>
    </div>
    <div v-else-if="isSolved" class="solved">
        Congratulations! You solved the puzzle!
    </div>
    
</template>

<style scoped>
.play-area {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 20px;
    width: 50%;
}

.selection-keyboard {
    display: flex;
    justify-content: center;
    align-items: center;
    flex: 6;
}

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

    flex: 6;
    max-width: 70%;
}

.solved {
    font-family: "ZCOOL XiaoWei", serif;
    font-weight: 400;
    font-style: normal;
    text-align: center;
    font-size: 3vh;
}

</style>