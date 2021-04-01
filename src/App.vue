<template>
  <div id="container">
    <Visualizer />
    <Buttons />
    <SongList />
    <p>Copyright &copy; 2021</p>
  </div>
</template>

<script>
import Visualizer from './components/Visualizer';
import Buttons from './components/Buttons';
import SongList from './components/SongList'

export default {
  name: 'App',
  components: {
    Visualizer,
    Buttons,
    SongList
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
    height: 800px;
    margin-top: -400px;
    width: 600px;
    margin-left: -300px;
    background-color: #585858;
    border-radius: 17px;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
}
</style>
