<template>
    <div class="btn" :id='type'>

    </div>
</template>

<script>
export default {
    name: 'Button',
    props: {
        type: String,
    }
}
</script>

<style>
    .btn#play{
        width: 75px;
        height: 75px;
        background-color: white;
        border-radius: 50%;
    }
    .btn#pause{
        width: 75px;
        height: 75px;
        background-color: purple;
        border-radius: 50%;
    }
    .btn#prev {
        width: 50px;
        height: 50px;
        background-color: white;
    }
    .btn#next {
        width: 50px;
        height: 50px;
        background-color: white;
    }
</style>