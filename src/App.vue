<template>
  <div id="app">
    <playlist-toolbar v-if="wpf" :playlist="wpf" />
    hi
    <button @click="loadPlaylist"></button>
    <div class="playlist-toolbar"></div>
    <div id="playlist"></div>

  </div>
</template>
<script>
  import WaveFormPlayList from "./main.js"
  export default {
    name: "playlist",
    data() {
      return {
        wpf: undefined,
        context: {},
      }
    },
    mounted() {

    },
    methods: {
      loadPlaylist() {
        this.context = new AudioContext();
        this.context.resume().then(() => {
          let WPF = WaveFormPlayList
          this.wpf = WPF({
            samplesPerPixel: 1000,
            waveHeight: 100,
            container: document.getElementById("playlist"),
            timescale: true,
            state: 'cursor',
            colors: {
              waveOutlineColor: '#E0EFF1'
            },
            controls: {
              show: true, //whether or not to include the track controls
              width: 200 //width of controls in pixels
            },
            zoomLevels: [500, 1000, 3000, 5000]
          })
          this.wpf.load([

            {
              "src": "DrumLoop.wav",
              "name": "drums",
            }
          ]).then(function () {

          })
          console.log('Playback resumed successfully');
        });

      }
    }



  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }

  #nav {
    padding: 30px;
  }

  #nav a {
    font-weight: bold;
    color: #2c3e50;
  }

  #nav a.router-link-exact-active {
    color: #42b983;
  }
</style>