<script setup>
import { ref, reactive, watch, nextTick, computed } from 'vue';

const props = defineProps({
    selectedCell: {
        type: Object,
        required: true,
        default: false,
    },
    parentIndex: {
        type: Number,
        required: true,
    },
    childIndex: {
        type: Number,
        required: true,
    },
    selectionMode: {
        type: String,
        required: true
    },
    numberSelection: {
        type: String,
        required: true,
    }
});

const emit = defineEmits(['cellClick', 'numberEntered']);

// Represents the number chosen for the cell
const chosenNumber = ref("");

const isSelected = computed(() => props.selectedCell.parent === props.parentIndex && props.selectedCell.child === props.childIndex);

// Tracks all candidate numbers chosen
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

// References to the DOM elements
const candidateGrid = ref(null);
const chosenBox = ref(null);

// Represents 
const cellIndexMapping = {
    "0": { row: 0, col: 0 },
    "1": { row: 0, col: 1 },
    "2": { row: 0, col: 2 },
    "3": { row: 1, col: 0 },
    "4": { row: 1, col: 1 },
    "5": { row: 1, col: 2 },
    "6": { row: 2, col: 0 },
    "7": { row: 2, col: 1 },
    "8": { row: 2, col: 2 },
};

const handleNormalMode = (key) => {
    if (/^[1-9]$/.test(key)) {
        chosenNumber.value = key;
    } else if (key === "Backspace" || key === "Delete" || key === "") {
        chosenNumber.value = "";
    }

    // Calculate row and column
    let mapping = cellIndexMapping[props.childIndex];
    let row = Math.floor(props.parentIndex / 3) * 3 + mapping.row;
    let col = (props.parentIndex % 3) * 3 + mapping.col;

    emit('numberEntered', { number: chosenNumber.value, row: row, col: col});
};

const handleCandidateMode = (key) => {
    if (/^[1-9]$/.test(key)) {
        chosenNumber.value = "";
        candidateNumbers[key] = !candidateNumbers[key];
    } else if (key === "Backspace" || key === "Delete" || key === "") {
        for (let i = 1; i <= 9; i++) {
            candidateNumbers[i] = false;
        }
    }
};

const handleKeyBoardPress = (e) => {
    const key = e.key;

    if (props.selectionMode === 'normal') {
        handleNormalMode(key);
    } else {
        handleCandidateMode(key);
    }
};

const handleMouseClick = (e, number) => {
    if (isSelected.value) {
        candidateNumbers[number] = !candidateNumbers[number];
    }
};

watch(chosenNumber,
    async (newValue) => {
        await nextTick();

        if (newValue === "") {
            candidateGrid.value.focus();
        } else {
            chosenBox.value.focus();
        }
    },
    {
    flush: "post",
    }
);

watch(() => props.numberSelection, () => {
    if (isSelected.value) {
        if (props.selectionMode === 'normal') {
            handleNormalMode(props.numberSelection);
        } else {
            handleCandidateMode(props.numberSelection);
        }
    }
});

</script>

<template>
    <div class="sudoku-cell" v-on:click="emit('cellClick', { parent: parentIndex, child: childIndex })" v-bind:class="{ 'sudoku-cell-selected': isSelected }">
        <!-- Render the value depending on if the player has a chosen number or not -->
        <div class="chosen-number-box" v-show="chosenNumber" tabindex="0" v-on:keydown="handleKeyBoardPress" ref="chosenBox">
            <svg class="chosen-number" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" width="100%" height="100%">
                <text x="50" y="50" font-family="Roboto, sans-serif" font-size="100" text-anchor="middle" dominant-baseline="central">
                    {{ chosenNumber }}
                </text>
            </svg>
        </div>

        <div class="candidate-number-grid" v-show="!chosenNumber" tabindex="0" v-on:keydown="handleKeyBoardPress" ref="candidateGrid">
            <div class="candidate-number-box" v-for="number in 9" v-bind:key="number" v-on:click="(e) => handleMouseClick(e, number)">
                <svg v-bind:class="[isSelected ? candidateNumbers[number] ? 'candidate-number-selected' : 'candidate-number-deselected' : candidateNumbers[number] ? 'cell-not-selected-number-selected' : 'cell-not-selected-number-not-selected']" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" width="100%" height="100%">
                    <text x="50" y="50" font-family="Roboto, sans-serif" font-size="100" text-anchor="middle" dominant-baseline="central">
                        {{ number }}
                    </text>
                </svg>
            </div>
        </div>        
    </div>
</template>

<style scoped>
    .sudoku-cell {
        background-color: white;
        box-sizing: border-box;
        aspect-ratio: 1 / 1;
        width: 100%;
        cursor: pointer;
    }

    .sudoku-cell-selected {
        background-color: #42B883;
    }

    .chosen-number-box {
        outline: none;
        width: 100%;
        height: 100%;
    }

    .candidate-number-grid {
        display: grid;
        grid-template-rows: repeat(3, 1fr);
        grid-template-columns: repeat(3, 1fr);
        outline: none;
    }

    .candidate-number-box {
        user-select: none; /* Prevents being able to select and control c/v something */
    }

    /* Fixes the descender issue */
    .candidate-number-deselected {
        display: block;
        opacity: 0;
        transition: opacity 0.2s ease-in;
    }

    .candidate-number-deselected:hover {
        opacity: 0.5;
    }

    .candidate-number-selected {
        display: block;
        opacity: 1;
    }

    .candidate-box-unselected {
        display: block;
        opacity: 0.5;
    }

    .cell-not-selected-number-selected {
        display: block;
        opacity: 0.5;
    }
    
    .cell-not-selected-number-not-selected {
        display: block;
        opacity: 0;
    }
</style>

        