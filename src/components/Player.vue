<template>
<div class="player">
  <Buttons
    :is-playing="isPlaying" 
    @prev-song="prev"
    @play-song="play"
    @pause-song="pause"
    @next-song="next"
  />
  <div class="playlist">
    <div class="song" v-for="song in songs" :key="song">
      <!-- <audio  controls>
        <source :src="song.src" />
      </audio> -->
      <span class="title">{{song.title}}</span>
    </div>
  </div>
</div>
</template>

<script>
import Buttons from './Buttons'
export default {
  name: "Player",
  components: {
    Buttons
  },
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        { title: 'Vesna prishla ochem bistro',
          src: require("../assets/audio1.mp3") 
        },
        {
          title: 'Happy song broke boy', 
          src: require("../assets/audio2.mp3") 
        },
        {
          title: 'Stepnoy rap',
          src: require("../assets/audio3.mp3") 
        },
      ],
      playback: new Audio(),
    };
  },
  methods: {
    play (song) {
      if (typeof song != 'undefined') {
        this.current = song;
        this.playback.src = this.current.src;
      }
      this.playback.play();
      this.isPlaying = true;
    },
    pause () {
      this.playback.pause();
      this.isPlaying = false;
    },
    prev () {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index]
      this.play(this.current);
    },
    next () {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }

      this.current = this.songs[this.index]
      this.play(this.current);
    }
  },
  created () {
    this.current = this.songs[this.index];
    this.playback.src = this.current.src;
  }
};
</script>

<style scoped>
.player {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  height: 400px;
}
.playlist {
  width: 550px;
  height: 300px;
  background: black;
  border-radius: 17px;
}
.title  {
  color: white;
}
</style>