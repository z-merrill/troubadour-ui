<template>
  <div v-cloak>
    <label>{{ recording.filename }}</label>
    <p><audio :src="recording.url" controls></audio></p>
  </div>
</template>

<script>
  export default {
    name: 'Playback',
    props: ['recording'],
    data: function () {
      return {
        azureAccountName: 'troubadour',
        sasString: 'se=2021-08-11&sp=rwdlac&sv=2018-03-28&ss=b&srt=sco&sig=uNR6ARsSupLnRXp66tc5WI73k8nml9L/tL0pZRlXAas%3D',
        containerName: '',
        containerUrl: ''
      }
    },
    mounted () {

    },
    methods: {
      buildAzureConfig (file, filename) {
        return {
          // baseUrl for blob file uri (i.e. http://<accountName>.blob.core.windows.net/<container>/<blobname>)
          baseUrl: `https://${this.accountName}.blob.core.windows.net/${this.containerName}/${filename}`,
          sasToken: this.sasString, // Shared access signature querystring key/value prefixed with ?,
          file: file, // File object using the HTML5 File API,
          progress: this.handleFileProgress(), // progress callback function,
          complete: this.handleFileComplete(), // complete callback function,
          error: this.handleFileError() // error callback function,
        }
      },
      handleFileProgress () {

      },
      handleFileComplete () {

      },
      handleFileError () {

      }
    }
  }
</script>
