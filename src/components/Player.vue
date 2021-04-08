<template>
<div class="player">
  <Visualizer 
  />
  <audio
  :src='playlist[index].src'
  type='audio/mp3'
  ref='myAudio'></audio>
  <p>{{current.title}}</p>
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
import Visualizer from './Visualizer';

export default {
  name: "Player",
  mounted: function() {
    this.myAudioPlayer = this.$refs.myAudio;
    this.setAnalyser();
    if (this.playback.isPlaying) {
      this.setAnalyser();
    }
    console.log(this.audioContextById);
  },
  props: {
    playlist: Array,
  },
  components: {
    Visualizer,
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
    };
  },
  methods: {
    play (song) {
      if (typeof song != 'undefined') {
        this.current = song;
        this.index = this.playlist.indexOf(song);
        this.playback.src = this.current.src;
      }
      this.playback.play();
      // console.log(this.myAudioPlayer)
      this.isPlaying = true;
      // this.myAudioPlayer = this.$refs.myAudio;
      // this.myAudioPlayer.play();
      this.allSoundsById[this.index] = this.playback;
      // console.log(this.allSoundsById);
      // this.setAnalyser()
      // console.log(this.allSoundsById[this.index]);
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
    createAudioContextObj () {
      const ctx = new AudioContext();
      const src = ctx.createMediaElementSource(this.myAudioPlayer);
      ctx.crossOrigin = 'anonymous';
      this.myAudioPlayer.crossOrigin = 'anonymous';
      const analyser = ctx.createAnalyser();
      src.connect(analyser);
      analyser.fftSize = 2048;
      analyser.connect(ctx.destination);
      const bufferLength = this.analyser.frequencyBinCount;
      const freqData = new Uint8Array(bufferLength);
      const audioContextObj = {
        freqData,
        analyser
      };
      console.log(audioContextObj);
      return audioContextObj;
    },
    setAnalyser () {
      Object.keys(this.allSoundsById).forEach(function(sound, id) {
        console.log(this.audioContextById[id])
        if (!this.audioContextById) {
          this.audioContextById[id] = this.createAudioContextObj();
        }
      })
    }
  },
  created () {
    this.current = this.playlist[this.index];
    this.playback.src = this.current.src;
    this.audioContextById = [];
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
.title  {
  color: white;
}
</style>