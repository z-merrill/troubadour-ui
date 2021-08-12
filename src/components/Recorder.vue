<template>
  <div v-cloak>
    <div class="form-group">
      <label for="title">Title</label>
      <input type="text" class="form-control" v-model="title" id="title" aria-describedby="title" placeholder="Enter title">
      <small id="title" class="form-text text-muted">Give your files a title - we'll number the takes.</small>
    </div>
    <div class="form-group" >
      <button ref="recordButton">{{ recordButtonText }}</button>
    </div>
    <div class="form-group">
      <label for="upload">or upload a file instead</label>
      <input id="upload" type="file" accept="audio/*" capture ref="recorder">
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Recorder',
    data: function () {
      return {
        title: 'my recording',
        recordButtonText: 'RECORD',
        recordedChunks: []
      }
    },
    mounted () {
      const recorder = this.$refs.recorder
      recorder.addEventListener('change', this.createAudioFile, false)
      navigator.mediaDevices.getUserMedia({ audio: true, video: false })
        .then(this.handleMicrophoneStream)
    },
    methods: {
      createAudioFile (event) {
        const file = event.target.files[0]
        const url = URL.createObjectURL(file)
        // Do something with the audio file.
        this.$refs.player.src = url
      },
      recordingFinished () {
        const url = URL.createObjectURL(new Blob(this.recordedChunks))
        this.$emit('recording-finished', { 'url': url, 'title': this.title })
      },
      dataAvailable (event) {
        if (event.data.size > 0) {
          this.recordedChunks.length = 0
          this.recordedChunks.push(event.data)
        }
      },
      handleMicrophoneStream (stream) {
        const self = this
        const options = {mimeType: 'audio/webm'}
        const mediaRecorder = new MediaRecorder(stream, options)

        mediaRecorder.addEventListener('dataavailable', this.dataAvailable)
        mediaRecorder.addEventListener('stop', this.recordingFinished)

        this.$refs.recordButton.addEventListener('click', function () {
          if (mediaRecorder.state === 'recording') {
            self.recordButtonText = 'RECORD'
            mediaRecorder.stop()
          } else {
            self.recordButtonText = 'STOP RECORDING'
            mediaRecorder.start()
          }
        })
      }
    }
  }
</script>
