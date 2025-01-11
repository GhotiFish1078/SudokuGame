<script setup>
import SudokuBoard from './components/SudokuBoard.vue';
</script>

<template>
	<div class="game-space">
		<SudokuBoard />
	</div>
	
</template>

<style scoped>
	.game-space {
		display: flex;

		justify-content: center;
	}
</style>
