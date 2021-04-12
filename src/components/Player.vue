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
    this.width = this.$refs.canvas.clientWidth;
    this.height = this.$refs.canvas.clientHeight;
    this.canvasCtx = this.$refs.canvas.getContext('2d');
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
      canvasCtx: null,
      audioCtx: null,
      width: null,
      height: null,
      barHeight: null,
      barWidth: null,
      barsCount: null,
    };
  },
  methods: {
    play (song) {
      if (typeof song != 'undefined') {
        this.current = song;
        this.index = this.playlist.indexOf(song);
        this.playback.src = this.current.src;
      }
      this.visualize(this.index);
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
    setAnalyser (audio) {
      const context = new AudioContext();
      const src = context.createMediaElementSource(audio);
      const analyser = context.createAnalyser();

      src.connect(analyser);
      analyser.fftsize = 2048;
      analyser.connect(context.destination);
      
      const bufferLength = analyser.frequencyBinCount;
      const freqData = new Uint8Array(bufferLength);
      const audioContextObj = {
        freqData, // note: at this time, this area is unpopulated!
        analyser
      }

      return audioContextObj;
    },
    createAudio (id) {
      if (!this.allSoundsById[id]) {
        let audio = new Audio();
        audio.src = this.playlist[id].src;
        audio.load();
        this.allSoundsById[id] = audio;
        if (!this.audioContextById[id]) {
          this.audioContextById[id] = this.setAnalyser(audio);
        }
      }
    },
    renderFrame () {
      const freqDataMany = []; // reset array that holds the sound data for given number of audio sources
      const agg = []; // reset array that holds aggregate sound data

      this.canvasCtx.clearRect(0, 0, this.width, this.height); // clear canvas at each frame
      this.canvasCtx.fillStyle = 'black';
      this.canvasCtx.fillRect(0, 0, this.width, this.height);
      // let audioContextArr = Object.values(this.audioContextById);

      // for each element in that array, get the *current* frequency data and store it
      this.audioContextById.forEach(function (audioContextObj) {
        let freqData = audioContextObj.freqData;
        audioContextObj.analyser.getByteFrequencyData(freqData); // populate with data
        freqDataMany.push(freqData);
      });

      if (this.audioContextById.length > 0) {
        // aggregate that data!
        for (let i = 0; i < freqDataMany[0].length; i++) {
          agg.push(0);
          freqDataMany.forEach(function (data) {
              agg[i] += data[i];
          });
        }

        let x = 0;


        for (let i = 0; i < (this.barsCount); i++) {
          this.barHeight = (agg[i] * 0.9);
          let y = (this.height - this.barHeight);
          this.canvasCtx.fillStyle = `#fff`;
          this.canvasCtx.fillRect(x, y, this.barWidth, this.barHeight);
          if (i < this.barsCount) {
              x += this.barWidth + 1;
          } else {
              this.barWidth += this.barWidth + 1;
              x += this.barWidth + 1;
          }
        }
      }
      requestAnimationFrame(this.renderFrame); // this defines the callback function for what to do at each frame
    },
    renderVisualizer () {
      // const canvasCtx = this.$refs.canvas.getContext('2d');
      this.barsCount = 300;
      this.barWidth = this.width / this.barsCount;
      this.renderFrame();
    },
    visualize (id) {
      this.createAudio(id);
      this.renderVisualizer();
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