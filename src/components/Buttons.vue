<template>
    <div class="buttons">
        <PrevBtn />
        <PlayBtn />
        <NextBtn />
    </div>
</template>

<script>
import PlayBtn from './PlayBtn'
import PrevBtn from './PrevBtn'
import NextBtn from './NextBtn'

export default {
    name: 'Buttons',
    components: {
        PlayBtn,
        PrevBtn,
        NextBtn
    }
}
</script>

<style scoped>
    .buttons {
        display: flex;
        justify-content: space-around;
        align-items: center;
        width: 250px;
        height: 75px;
        margin: auto;
        background-color: black;
    }
</style>