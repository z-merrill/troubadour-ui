<template>
  <div class="container-fluid" id="login">
    <br/>
    <div class="row justify-content-center">
      <form>
        <div class="form-group">
          <input type="text" name="handle" v-model="input.handle" placeholder="@handle" />
          <small class="form-text text-muted">your desired handle</small>
        </div>
        <div class="form-group">
          <p><span class="floating-placeholder" v-if="badEmail">please enter a valid email address</span></p>
          <input type="email" name="email" v-model="input.email" required placeholder="ass@butt.com" @change="validateEmail" />
          <small class="form-text text-muted">an email you own</small>
        </div>
        <div class="form-group">
          <input type="password" name="password" v-model="input.password" placeholder="hunter2" />
          <small class="form-text text-muted">a very strong password</small>
        </div>

        <button type="button" v-on:click="signup()" :disabled='badEmail'>signup</button> or 
        <router-link to="/login">login</router-link>
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
          password: '',
          email: ''
        },
        badEmail: false
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
      },
      validateEmail () {
        if (/^\w+([.-]?\w+)*@\w+([.-]?\w+)*(.\w{2,3})+$/.test(this.input.email)) {
          this.badEmail = false
        } else {
          this.badEmail = true
        }
      }
    }
  }
</script>