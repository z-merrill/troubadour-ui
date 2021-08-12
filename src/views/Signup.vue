<template>
  <div class="container-fluid mt-4" id="login">
    <div class="row justify-content-center"><h1 class="display-4">signup</h1></div>
    <br/>
    <div class="row justify-content-center">
      <form>
        <div class="form-group">
          <input type="text" name="handle" v-model="input.handle" placeholder="@handle" />
          <small class="form-text text-muted">your desired handle</small>
        </div>
        <div class="form-group">
          <input type="email" name="email" v-model="input.email" placeholder="ass@butt.com" />
          <small class="form-text text-muted">an email you own</small>
        </div>
        <div class="form-group">
          <input type="password" name="password" v-model="input.password" placeholder="hunter2" />
          <small class="form-text text-muted">a very strong password</small>
        </div>

        <button type="button" v-on:click="signup()">Signup</button>
      </form>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'Signup',
    data () {
      return {
        input: {
          handle: '',
          password: ''
        }
      }
    },
    methods: {
      signup () {
        if (this.input.handle !== '' && this.input.password !== '' && this.input.email !== '') {
          axios
            .post('http://localhost:8081/api/auth/signup', this.input)
            .then((response) => {
              this.$emit('logged-in', { 'header': response.headers.authorization, 'handle': response.data.handle })
              this.$router.replace({ name: 'Files' })
            }, (error) => {
              console.log(error)
            })
        } else {
          console.log('A username, email, and password must be present')
        }
      }
    }
  }
</script>