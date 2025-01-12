<script setup>

</script>

<template>
    <div class="pause-menu">
        <span>
            Game Paused
        </span>
        <button class="button" type="button">
            Resume Game
        </button>
    </div>
</template>

<style scoped>
    .pause-menu {
        border-width: 1px;
        border-color: black;
        border-style: solid;
        border-radius: 5px;

        height: 50vh;
        width: 75vw;

        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;

        box-shadow: 0 0 5px black;

        font-size: 1vh;
        margin: auto;
    }

    .button {
        background-color: black;

        color: white;

        font-weight: bold;
        padding: 3vh 3vw 3vh 3vw;

        border-radius: 25vh;
        border-style: none;
        cursor: pointer;
        outline: none;

        font-size: 200%;
    }

    .button span {
        width: 100%;
        height: 100%;
        text-align: center;
    }

    span {
        font-size: 200%;
        font-weight: bold;
        font-family: Arial, sans-serif;
        padding: 5vh 5vw 5vh 5vw;
    }
</style>