<template>
  <div><br>
    <div class="register-container container col-4 shadow-lg p-5">
        <h1 class="text-center">Sign Up for free</h1>
        <div class="card bg-transparent">
          <form @submit.prevent="register" class="p-2">
             <div class="form-group">
              <label for="exampleInputFullName">Full Name</label>
              <input v-model="registUser.fullName" type="fullName" class="form-control" id="exampleInputFullName" >
   
            </div>
            <div class="form-group">
              <label for="exampleInputEmail1">Email address</label>
              <input v-model="registUser.email" type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp">
   
            </div>
            <div class="form-group">
              <label for="exampleInputPassword1">Password</label>
              <input v-model="registUser.password" type="password" class="form-control" id="exampleInputPassword1">
   
            </div>
            <button type="submit" class="btn btn-primary">Register</button>
          </form>  
        </div>
        <br>
         <p class="text-center">
              Already have an account?
              <a href="#" @click="changePage">Login here</a>
            </p>
      </div>
  </div>
</template>

<script>
export default {
  name : "register-form-component",
  data(){
    return {
      registUser : {
        email : '',
        password : ''
      }
    }
  },
  methods :{
     changePage(){
      // console.log('login vue');
      this.$emit("halamanLogin","login page")
    },
    register(){
      // console.log(this.registUser, 'mashook');
      this.$emit("registUser", this.registUser)
    }
  }
}
</script>

<style>

</style>