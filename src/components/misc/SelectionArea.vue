<script setup>
import { onMounted, onUnmounted, ref } from 'vue';

// selection mode is normal by default
const mode = ref("normal");
const mouseDownIdx = ref(-1);

// Send event to the main board
const emit = defineEmits(['selectionModeUpdate', 'selectionChoiceUpdate']);

const handleClick = (modeSelection) => {
    mode.value = modeSelection;
    emit('selectionModeUpdate', mode.value);
};

const handleChoiceClick = (selection) => {
    emit('selectionChoiceUpdate', selection);
};

function handleGlobalMouseUp() {
    mouseDownIdx.value = -1;
}

onMounted(() => {
  window.addEventListener('mouseup', handleGlobalMouseUp);
});

onUnmounted(() => {
  window.removeEventListener('mouseup', handleGlobalMouseUp);
});

</script>

<template>
    <div class="selection-area">
        <div class="selection-mode">
            <button v-on:click="handleClick('normal')" v-bind:class="{ 'selected': mode === 'normal'}"> Normal </button>
            <button v-on:click="handleClick('candidate')" v-bind:class="{ 'selected': mode === 'candidate'}"> Candidate </button>
        </div>

        <div class="selection-box">
            <div class="selection-value" v-bind:class="{'mouse-down': i === mouseDownIdx}" v-for="i in 9" :key="i" v-on:mousedown="mouseDownIdx = i" v-on:mouseup="mouseDownIdx = -1" v-on:click="handleChoiceClick(`${i}`)">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" width="100%" height="100%">
                    <text x="50" y="50" font-family="Roboto, sans-serif" font-size="100" text-anchor="middle" dominant-baseline="central">
                        {{ i }}
                    </text>
                </svg>
            </div>
            
            <div class="delete-button" v-bind:class="{'mouse-down': 0 === mouseDownIdx}" v-on:click="handleChoiceClick('')" v-on:mousedown="mouseDownIdx = 0" v-on:mouseup="mouseDownIdx = -1">
                <svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 100 100">
                    <line x1="10" y1="10" x2="90" y2="90" stroke="black" stroke-width="10" />
                    <line x1="90" y1="10" x2="10" y2="90" stroke="black" stroke-width="10" />
                </svg>
            </div>
        </div>

        
    </div>
</template>

<style scoped>
    .selection-area {
        width: 50%;
    }

    .selection-mode {
        display: flex;
        height: 10%;
    }

    .selection-mode button {
        background-color: white;
        cursor: pointer;
        padding-left: 10%;
        padding-right: 10%;
        width: 50%;
        box-sizing: border-box;
        border-style: solid;
        border-color: black;
        border-width: 1px;

        font-weight: bold;
        font-size: larger;

        opacity: 50%;
    }

    .selection-mode button.selected {
        background-color: black;
        opacity: 100%;
        color: white;
    }

    button:first-child {
        border-top-left-radius: 5px;
        border-bottom-left-radius: 5px;
    }

    button:last-child {
        border-top-right-radius: 5px;
        border-bottom-right-radius: 5px;
    }

    .selection-box {
        display: grid;
        grid-template-rows: repeat(4, 1fr);
        grid-template-columns: repeat(3, 1fr);
        gap: 10px;
        padding-top: 10px;
        user-select: none;
    }

    .selection-value {
        border-style: solid;
        border-color: grey;
        border-width: 1px;
        background-color: lightgray;
        border-radius: 5px;
    }

    .delete-button {
        grid-area: 4 / 1 / 5 / 4;
        border-style: solid;
        border-color: grey;
        border-width: 1px;
        
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: lightgray;
        border-radius: 5px;
    }

    .selection-value:hover,
    .delete-button:hover {
        cursor: pointer;
    }

    .mouse-down {
        opacity: 50%;
        background-color: white;
    }

</style>