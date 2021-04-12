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
      // if (!this.allSoundsById[this.index]) {
      //   this.allSoundsById[this.index] = this.audio;
      // }
      if (typeof song != 'undefined') {
        this.current = song;
        this.index = this.playlist.indexOf(song);
        this.playback.src = this.current.src;
      }
      this.createAudio(this.index);
      console.log(this.allSoundsById);
      this.playback.play();
      this.isPlaying = true;
      this.audio = this.$refs.myAudio;
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
    setAnalyser () {
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
    loadContext () {
      Object.keys(this.allSoundsById).forEach(function(sound, id) {
        if (!this.audioContextById[id]) {
          this.audioContextById[id] = this.setAnalyser();
        }
      })
    },
    createAudio (id) {
      if (!this.allSoundsById[id]) {
        let audio = new Audio();
        audio.src = this.playlist[id].src;
        audio.load();
        this.allSoundsById[id] = audio;
        this.loadContext();
      }
    }
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