<template>
    <div class="buttons">
        <Button id='prev' @click="$emit('prev-song')"/>
        <Button id='play' v-if="!isPlaying" @click="$emit('play-song')"/>
        <Button id='pause' v-else @click="$emit('pause-song')"/>
        <Button id='next' @click="$emit('next-song')"/>
    </div>
</template>

<script>
import Button from './Button'

export default {
    name: 'Buttons',
    props: {
        isPlaying: Boolean,
    },
    components: {
        Button
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
        background-color: black;
    }
</style>