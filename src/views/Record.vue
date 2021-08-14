<template>
  <div class="container-fluid mt-4" v-cloak>
    <div class="row justify-content-center">
      <h1 class="display-4">record something</h1>
    </div>
    <div class="row justify-content-center">
      <form>
        <Recorder @recording-finished="recordingFinished" />
        <div class="form-group" v-for="recording in recordings" :key="recording.filename">
          <Playback :recording="recording" :token="token" />
        </div>

        <router-link to="/files" custom v-slot="{ navigate }">
          <button @click="navigate" role="link" class="btn btn-primary">all done</button>
        </router-link>
      </form>
    </div>
  </div>
</template>

<script>
  import Recorder from '@/components/Recorder'
  import Playback from '@/components/Playback'

  export default {
    name: 'Record',
    components: {
      Recorder,
      Playback
    },
    props: ['user', 'token'],
    data: function () {
      return {
        recordButtonText: 'RECORD',
        recordings: []
      }
    },
    mounted () {
      if (!this.user) {
        this.$router.push({ name: 'Login' })
      }
    },
    methods: {
      recordingFinished (payload) {
        this.recordings.push({url: payload.url, filename: `${payload.title}-Take ${this.recordings.length + 1}.ogg`})
      }
    }
  }
</script>
