<script setup>
import GamePausedPopup from './components/misc/GamePausedPopup.vue';
import SudokuBoard from './components/board/SudokuBoard.vue';
import SelectionArea from './components/misc/SelectionArea.vue';

</script>

<template>
	<SelectionArea />

	<div class="game-space">
		<SudokuBoard />
	</div>

	<GamePausedPopup />
	
</template>

<style scoped>
	.game-space {
		display: flex;
		justify-content: center;
	}
</style>
