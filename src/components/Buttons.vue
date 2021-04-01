<template>
    <div class="buttons">
        <Button id='prev' />
        <Button id='play' />
        <Button id='next' />
    </div>
</template>

<script>
import Button from './Button'

export default {
    name: 'Buttons',
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