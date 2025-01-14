<script setup>

const emit = defineEmits(['submit', 'reset', 'newPuzzle']);

</script>

<template>
    <div class="options">
        <button class="submit" v-on:click="emit('submit')">
            Submit
        </button>
        <button class="new-puzzle" v-on:click="emit('newPuzzle')">
            New Random Puzzle
        </button>
    </div>
</template>

<style scoped>
    .options {
        display: flex;
        justify-content: center;
        align-items: center;
        padding-top: 3vh;
    }

    button {
        padding: 10px;
        margin-left: 10px;
        margin-right: 10px;

        font-family: "ZCOOL XiaoWei", serif;
		font-weight: 400;
		font-style: normal;
		text-align: center;
		font-size: 3vh;
    }
</style>