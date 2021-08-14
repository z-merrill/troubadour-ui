<template>
  <div v-cloak>
    <label>{{ recording.filename }}</label>
    <p><audio :src="recording.url" controls></audio></p>
    <a href="#" @click.prevent="upload" >upload file</a>
  </div>
</template>

<script>
  import axios from 'axios'
  export default {
    name: 'Playback',
    props: ['recording', 'token'],
    data: function () {
      return {
        file: ''
      }
    },
    async mounted () {
      this.file = await fetch(this.recording.url)
        .then(r => r.blob())
        .then(blobFile => new File([blobFile], this.recording.filename, { type: "audio/ogg" }))
    },
    methods: {
      upload () {
        let formData = new FormData()
        formData.append('file', this.file)
        axios
          .post('http://localhost:8081/api/files/upload', formData, { headers: { Authorization: this.token } })
          .then((response) => {
            console.log('SUCCESS!!')
            console.log(response)
          }, (error) => {
            console.log('FAILURE!!')
            console.log(error)
          })
      }
    }
  }
</script>
