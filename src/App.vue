<script setup>
import SudokuBoard from './components/board/SudokuBoard.vue';

</script>

<template>
	<div class="title-space">
		Welcome to my Sudoku Site!
	</div>
	<div class="game-space">
		<SudokuBoard />
	</div>
	<div class="remark-space">
	</div>
</template>

<style scoped>
	.title-space {
		font-family: "ZCOOL XiaoWei", serif;
		font-weight: 400;
		font-style: normal;
		text-align: center;
		font-size: 10vh;
		padding-bottom: 5vh;
	}

	.game-space {
		display: flex;
		justify-content: center;
	}
</style>
