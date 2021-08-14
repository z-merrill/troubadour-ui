<template>
  <div id="app">
    <div id="nav" class="centered">
      <div v-if="!activeUser">
        <router-link to="/">Home</router-link> |
        <router-link to="/login">Login</router-link> |
        <router-link to="/signup">Signup</router-link>
      </div>
      <div v-else>
        <a href="#" @click.prevent="" >@{{ activeUser.handle }}</a> |
        <router-link to="/record">Record</router-link> |
        <router-link to="/files">Files</router-link> |
        <a href="#" @click.prevent="logout" >Logout</a>
      </div>
    </div>
    <transition name="fade">
      <router-view @logged-in="login" v-bind:user="activeUser" v-bind:token="activeToken" />
    </transition>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.centered {
  text-align: center;
}

#nav {
  padding-top: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}

img {
  padding-top: 30px;
}

.fade-enter-active {
  transition: opacity .5s;
}

.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>

<script>
import axios from 'axios'
export default {
  name: 'app',
  data () {
    return {
      activeUser: null,
      activeToken: null
    }
  },
  async created () {
    await this.refreshActiveUser()
  },
  watch: {
    // everytime a route is changed refresh the activeUser
    '$route': 'refreshActiveUser'
  },
  methods: {
    login (payload) {
      axios
        .get(`http://localhost:8081/api/users/${payload.handle}`, { headers: { Authorization: payload.header } })
        .then((response) => {
          this.activeUser = response.data
          this.activeToken = payload.header
          this.$router.replace({ name: 'Files' })
        }, (error) => {
          console.log(error)
        })
    },
    async refreshActiveUser () {
      if (this.activeUser) {
        axios
          .get(`http://localhost:8081/api/users/${this.activeUser.handle}`, { headers: { Authorization: this.activeToken } })
          .then((response) => {
            this.activeUser = response.data
          }, (error) => {
            console.log(error)
          })
      }
    },
    async logout () {
      this.activeUser = null
      this.$router.replace({ name: 'Login' })
    }
  }
}
</script>
