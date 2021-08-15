<template>
  <div class="container-fluid mt-4" v-cloak>
    <div class="row justify-content-center"><h1 class="display-4">my files</h1></div>
    <div class="row justify-content-center" v-if="user">
      <form>
        <div class="form-group" v-for="(file, index) in user.files" :key="file.id">
          <Playback :recording="file" :token="token" :index="index" :uploaded=true @deleted="removeFile"/>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
  import Playback from '@/components/Playback'
  export default {
    name: 'Files',
    props: ['user', 'token'],
    components: {
      Playback
    },
    mounted () {
      // if (this.user === null) {
      //   this.$router.push({ name: 'Login' })
      // }
    },
    methods: {
      removeFile (payload) {
        this.user.files.splice(payload.index, 1)
      }
    }
  }
</script>
