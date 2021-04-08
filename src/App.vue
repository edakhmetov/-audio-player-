<template>
  <div id="container">
    <Player 
      :playlist='playlist'
    />
    <p>Copyright &copy; 2021</p>
  </div>
</template>

<script>
import Player from './components/Player'

export default {
  name: 'App',
  components: {
    Player
  },
  data() {
    return {
      playlist: [
        { title: 'Vesna prishla ochem bistro',
          src: require("./assets/audio1.mp3") 
        },
        {
          title: 'Happy song broke boy', 
          src: require("./assets/audio2.mp3") 
        },
        {
          title: 'Stepnoy rap',
          src: require("./assets/audio3.mp3") 
        },
      ]
    }
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
