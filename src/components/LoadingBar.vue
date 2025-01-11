<script setup>
import katex from 'katex';
import { ref } from 'vue';

const string = katex.renderToString("e^{i \\pi } = -1", {
    throwOnError: true,
    output: "mathml",
});

const tips = ref([
    "Ensure each square is filled!",
    "Running on Bogo sort...",
    "To be honest, even I don't know when this will load...",
    string,
    "Pootis Pow!"
]);

console.log(tips);

</script>

<template>
    <div class="loading-bar-box">
        <div class="progress-bar">
            
        </div>
        <div class="tool-tip" v-html="tips[3]">

        </div>
    </div>
</template>

<style scoped>
.loading-bar-box {
    border-color: black;
    border-style: solid;
    border-width: 2px;
}

.progress-bar {
    margin: 20px;
    border-color: black;
    border-style: solid;
    border-width: 2px;
    height: 30px;
}

.tool-tip {
    text-align: center;
}

</style>