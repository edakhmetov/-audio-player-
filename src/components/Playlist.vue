<template>
  <div class="Playlist">
    <div class="player" v-for="song in songs" :key="song">
      <audio  controls>
        <source :src="song.source" />
      </audio>
      <span class="title">{{song.title}}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "Playlist",
  data() {
    return {
      songs: [
        { title: 'Vesna prishla ochem bistro',
          source: require("../assets/audio1.mp3") 
        },
        {
          title: 'Happy song broke boy', 
          source: require("../assets/audio2.mp3") 
        },
        {
          title: 'Stepnoy rap',
          source: require("../assets/audio3.mp3") 
        },
      ],
    };
  },
};
</script>

<style scoped>
.Playlist {
  width: 550px;
  height: 300px;
  background: black;
  border-radius: 17px;
}
.title  {
  color: white;
}
</style>