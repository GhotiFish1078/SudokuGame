<script setup>
import { ref } from 'vue';
import EditableSudokuCell from './EditableSudokuCell.vue';
import NonEditableSudokuCell from './NonEditableSudokuCell.vue';

const props = defineProps({
    values: {
        type: Array,
        required: true,
    },
    selectedCell: {
        type: Object,
        required: true,
        default: false,
    },
    parentIndex: {
        type: Number,
        required: true,
    },
    selectionMode: {
        type: String,
        required: true,
    },
    numberSelection: {
        type: String,
        required: true,
    }
});

const emit = defineEmits(['reEmitEvent', 'reEmitNumber']);

const isSelected = ref(false);

</script>

<template>
    <div class="nine-block" >
        <template v-for="(i, index) in 9">
            <EditableSudokuCell v-if="!props.values[index]" v-bind="{'selected-cell': selectedCell, 'parent-index': parentIndex, 'child-index': index, 'selection-mode': selectionMode, 'number-selection': numberSelection }" v-on:cell-click="(e) => emit('reEmitEvent', e)" v-on:number-entered="(e) => emit('reEmitNumber', e)"/>
            <NonEditableSudokuCell v-else v-bind="{'cell-number': props.values[index]}"/>
        </template>
    </div>
</template>

<style scoped>
    .nine-block {
        display: grid;
        grid-template-rows: repeat(3, 1fr);
        grid-template-columns: repeat(3, 1fr);

        aspect-ratio: 1 / 1;
        gap: 1px;
        background-color: black;
    }
</style>