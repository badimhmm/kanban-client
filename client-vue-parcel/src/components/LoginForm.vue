<template>
  <div><br>
    <div class="login-container container col-4 shadow-lg p-5">
        <h1 class="text-center">User Login</h1>

        <div class="card bg-transparent">
          <form @submit.prevent="login" class="p-2">
            <div class="form-group">
              <label for="exampleInputEmail1"><strong>Email address</strong></label>
              <input v-model="email" type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
            </div>
            <div class="form-group">
              <label for="exampleInputPassword1"><strong>Password</strong></label>
              <input v-model="password" type="password" class="form-control" id="exampleInputPassword1">
            </div>
            <button type="submit" class="btn btn-primary">Login</button>
          </form>
         
        </div><br>
         <button v-google-signin-button="clientId" class="btn btn-danger"> Continue with Google</button>  
        <br><br>
        <p class="text-center">
              Don't have account?
              <a href="#" @click="changePage">Sign up here</a>
            </p>
      </div>
  </div>
</template>

<script>
import GoogleSignInButton from 'vue-google-signin-button-directive'

export default {
  name : "login-form-component",
  props: ['message'], 
  methods:{
    changePage(){
      // console.log('login vue');
      this.$emit("halamanRegister","register page")
    },
    login(){
      // console.log('cek disini', this.email, this.password )
      this.$emit('v-on:login="login"', {email: this.email, password: this.password})
    },
    OnGoogleAuthSuccess (idToken) {
      this.$emit('googleToken', idToken)
    },
    OnGoogleAuthFail (error) {
      console.log(error)
    }
  },
  data(){
    return {
     
        email : '',
        password : '',
 
      clientId : '920456466156-eu3p58i5g9rub7sc3c8rbsag8f9spd2q.apps.googleusercontent.com'
    }
  },
}
</script>

<style>

</style>