<template>
  <div class="container-fluid mt-4">
    <h1 class="h1">Projects</h1>
    <p>Only authenticated users should see this page</p>
  </div>
</template>

<script>
  export default {
    name: 'Projects',
    props: ['user'],
    data () {
      return {
        projects: []
      }
    },
    mounted () {
      if (!this.user) {
        this.$router.push({ name: 'Login' })
      }
    },
    methods: {

    }
  }
</script>