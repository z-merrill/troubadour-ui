<template>
  <div v-cloak>
    <hr/>
    <label>{{ recording.name }}</label>
    <p><audio :src="recording.url" controls></audio></p>
    <font-awesome-icon v-if="isUploaded" icon="cloud" />
    <a href="#" @click.prevent="upload" v-else>upload file</a> | 
    <a href="#" @click.prevent="deleteFile"><font-awesome-icon icon="trash" /></a>
  </div>
</template>

<script>
  import axios from 'axios'
  export default {
    name: 'Playback',
    props: ['recording', 'token', 'index', 'uploaded'],
    data: function () {
      return {
        file: '',
        isUploaded: null,
        url: ''
      }
    },
    async mounted () {
      this.file = await fetch(this.recording.url)
        .then(r => r.blob())
        .then(blobFile => new File([blobFile], this.recording.name, { type: "audio/ogg" }))
      this.isUploaded = this.uploaded
      this.url = this.recording.url
    },
    methods: {
      upload () {
        let formData = new FormData()
        formData.append('file', this.file)
        axios
          .post('http://localhost:8081/api/files/upload', formData, { headers: { Authorization: this.token } })
          .then((response) => {
            this.recording.id = response.data.id
            this.isUploaded = true
            this.url = response.data.url
          }, (error) => {
            console.log('FAILURE!!')
            console.log(error)
          })
      },
      deleteFile () {
        if (this.isUploaded) {
          axios
            .delete(`http://localhost:8081/api/files/${this.recording.id}`, { headers: { Authorization: this.token } })
            .then((response) => {
            console.log(response)
          },
          (error) => {
              console.log('FAILURE!!')
              console.log(error)
          })
        }
        this.$emit('deleted', { 'index': this.index })
      }
    }
  }
</script>
