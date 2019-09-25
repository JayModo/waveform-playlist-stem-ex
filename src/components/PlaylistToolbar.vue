<style>
  .playlist-toolbar .buttons button {
    margin: 1rem;
  }
</style>
<template>
  <div class="playlist-toolbar">
    <div class="buttons">
      <button :disabled="recording" @click="emitter.emit('record');recording = true;">Record</button>
      <button @click="emitter.emit('play')">Play</button>
      <button @click="emitter.emit('pause')">Pause</button>
      <button @click="emitter.emit('stop');recording = false;">Stop</button>
      <button @click="emitter.emit('startaudiorendering', 'wav');">Save New Track</button>
    </div>
  </div>
</template>
<script>
  export default {
    name: "playlist-toolbar",
    props: ['playlist'],
    data() {
      return {
        emitter: undefined,
        recording: false,
      };
    },
    computed: {
      // ...mapGetters([
      //   'newTrack'
      // ])
    },

    mounted() {
      const { gotStream, logError } = this;
      const constraints = {
        audio: true
      };
      // Create emitter

      debugger
      this.emitter = this.playlist.getEventEmitter();
      this.emitter.on("audiorenderingfinished", (type, blob) => {
        // const data = new FormData();
        // let config;
        // data.append("audio", blob, "new audio");
        // config = {
        //   headers: {
        //     "Content-Type": `multipart/form-data; boundary=${data._boundary}`
        //   }
        // };
        // this.createTrack(data, config);
      });
      this.playlist.initExporter();
      // Start media stream.

      if (navigator.mediaDevices) {
        navigator.mediaDevices
          .getUserMedia(constraints)
          .then(gotStream)
          .catch(logError);
      } else if (navigator.getUserMedia && "MediaRecorder" in window) {
        navigator.getUserMedia(constraints, gotStream, logError);
      }
    },
    methods: {
      // ...mapActions([
      //   'createTrack'
      // ]),
      gotStream(stream) {
        this.playlist.initRecorder(stream);
      },
      logError(e) {
        console.error(e);
      }
    }
  }
</script>