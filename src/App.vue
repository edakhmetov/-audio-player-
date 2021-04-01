<template>
  <div id="container">
    <Visualizer />
    <Buttons />
  </div>
</template>

<script>
import Visualizer from './components/Visualizer';
import Buttons from './components/Buttons';

export default {
  name: 'App',
  components: {
    Visualizer,
    Buttons
  }
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  background-color: black;
}

#container {
    position: absolute;
    left: 50%;
    top: 50%;
    height: 700px;
    margin-top: -350px;
    width: 600px;
    margin-left: -300px;
    background-color: #585858;
    border-radius: 17px;
}
</style>
