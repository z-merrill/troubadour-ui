<template>
  <div class="container-fluid" id="login">
    <div class="row justify-content-center">
      <form>
        <div class="form-group">
          <input type="text" name="handle" v-model="input.handle" placeholder="@handle" />
          <small class="form-text text-muted">your handle</small>
        </div>
        <div class="form-group">
          <input type="password" name="password" v-model="input.password" placeholder="hunter2" />
          <small class="form-text text-muted">your password</small>
        </div>

        <button type="button" v-on:click="login()">login</button> or
        <router-link to="/signup">signup</router-link>
      </form>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'Login',
    data () {
      return {
        input: {
          handle: '',
          password: ''
        }
      }
    },
    methods: {
      login () {
        if (this.input.handle !== '' && this.input.password !== '') {
          axios
            .post('http://localhost:8081/api/auth/login', this.input)
            .then((response) => {
              this.$emit('logged-in', { 'header': response.headers.authorization, 'handle': response.headers.handle })
              this.$router.replace({ name: 'Files' })
            }, (error) => {
              console.log(error)
            })
        } else {
          console.log('A username and password must be present')
        }
      }
    }
  }
</script>
