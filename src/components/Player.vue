<template>
<div class="player">
  <div class="visualizer">
    <canvas width="550" height="300" ref="canvas">
    </canvas>
  </div>

  <audio
  :src='playlist[index].src'
  type='audio/mp3'
  ref='myAudio'
  controls>
  </audio>
    <p class="title">{{current.title}}</p>


  <Buttons
    :is-playing="isPlaying" 
    @prev-song="prev"
    @play-song="play"
    @pause-song="pause"
    @next-song="next"
  />
  <div class="songPlaylist">
    <div class="song" v-for="song in playlist" :key="song">
      <button @click="play(song)">
        {{song.title}}
      </button>
    </div>
  </div>
</div>
</template>

<script>
import Buttons from './Buttons';

export default {
  name: "Player",
  mounted: function() {
    this.myAudioPlayer = this.$refs.myAudio;
    // this.loadElements();
  },
  props: {
    playlist: Array,
  },
  components: {
    Buttons
  },
  data() {
    return {
      myAudioPlayer: null,
      current: {},
      index: 0,
      isPlaying: false,
      playback: new Audio(),
      allSoundsById: [],
      audioContextById: [],
      analyser: null,
      audio: null,
      ctx: null,
      audioCtx: null,
    };
  },
  methods: {
    play (song) {
      if (typeof song != 'undefined') {
        this.current = song;
        this.index = this.playlist.indexOf(song);
        this.playback.src = this.current.src;
      }
      // console.log(this.myAudioPlayer);
      this.playback.play();
      this.isPlaying = true;
      this.loadElements();
      this.audio = this.$refs.myAudio;
      this.allSoundsById[this.index] = this.playback;
      // console.log(this.audio, this.current);
      // this.audioContextById[this.index] = this.setAnalyser();
      console.log(this.allSoundsById);
      // if (this.audioContextById[this.index] != this.audioCtx) {
      //   this.setAnalyser();
      //   this.audioContextById[this.index] = this.audioCtx;
      // }
      // this.setAnalyser();
      // console.log(this.audioCtx);
      // Object.keys(this.allSoundsById).forEach(function () {
        // condition to avoid creating duplicate context. the visualizer won't break without it, but you will get a console error.
        // if (!this.audioContextById[this.index]) {
        //     this.audioContextById[this.index] = this.setAnalyser();
        // }
      // });
      // console.log(this.audioContextById);
    },
    pause () {
      this.playback.pause();
      this.isPlaying = false;
    },
    prev () {
      this.index--;
      if (this.index < 0) {
        this.index = this.playlist.length - 1;
      }
      this.current = this.playlist[this.index]
      this.play(this.current);
    },
    next () {
      this.index++;
      if (this.index > this.playlist.length - 1) {
        this.index = 0;
      }
      this.current = this.playlist[this.index];
      this.play(this.current);
    },
    loadElements () {
      this.audio = this.$refs.myAudio;
      this.audio.load();
      this.ctx = this.$refs.canvas;
    },
    setAnalyser: function () {
      this.audioCtx = this.audioCtx || new AudioContext();
      this.analyser = this.analyser || this.audioCtx.createAnalyser();
      const src = this.audioCtx.createMediaElementSource(this.audio);

      src.connect(this.analyser);
      this.analyser.fftsize = 2048;
      this.analyser.connect(this.audioCtx.destination);
      
      const bufferLength = this.analyser.frequencyBinCount;
      const freqData = new Uint8Array(bufferLength);
      const analyser = this.analyser;
      const audioContextObj = {
        freqData, // note: at this time, this area is unpopulated!
        analyser
      }

      return audioContextObj;
    },
  },
  created () {
    this.current = this.playlist[this.index];
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
  height: 750px;
}
.songPlaylist {
  width: 550px;
  height: 300px;
  background: black;
  border-radius: 17px;
}
.visualizer {
        width: 550px;
        height: 300px;
        background-color: black;
        border-radius: 17px;
}
.title  {
  color: white;
}
</style>