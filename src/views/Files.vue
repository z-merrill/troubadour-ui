<template>
  <div class="container-fluid mt-4" v-cloak>
    <div class="row justify-content-center"><h1 class="display-4">my files</h1></div>
    <div class="row justify-content-center">
      <ul id="fileList" v-if="user">
        <li v-for="file in user.files" :key="file.id">
          {{ file.name }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'Files',
    props: ['user'],
    mounted () {
      if (this.user === null) {
        this.$router.push({ name: 'Login' })
      }
    }
  }
</script>
