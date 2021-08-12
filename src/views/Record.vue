<template>
  <div class="container-fluid mt-4" v-cloak>
    <div class="row justify-content-center"><h1 class="display-4">record some things</h1></div>
    <div class="row justify-content-center">
      <form>
        <Recorder @recording-finished="recordingFinished" />
        <div class="form-group" v-for="recording in recordings" :key="recording.filename">
          <Playback :recording="recording" />
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
    props: ['user'],
    data: function () {
      return {
        recordButtonText: 'RECORD',
        recordings: [],
        azureAccountName: 'troubadour',
        sasString: 'se=2021-08-11&sp=rwdlac&sv=2018-03-28&ss=b&srt=sco&sig=uNR6ARsSupLnRXp66tc5WI73k8nml9L/tL0pZRlXAas%3D',
        containerName: '',
        containerUrl: ''
      }
    },
    mounted () {
      if (!this.user) {
        this.$router.push({ name: 'Login' })
      }
    },
    methods: {
      recordingFinished (payload) {
        this.recordings.push({url: payload.url, filename: `${payload.title}-Take ${this.recordings.length + 1}.wav`})
      }
    }
  }
</script>
