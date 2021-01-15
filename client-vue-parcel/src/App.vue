<template>
  <div>
    <navbar-component :currentPage="currentPage" @halamanLogin="logout" ></navbar-component>
    <login-form-component v-if="currentPage == 'login page'" 
      @halamanRegister="changePage"
      v-on:login="login"
      @googleToken="googleSignIn"
    ></login-form-component>
    <register-form-component v-else-if="currentPage == 'register page'" @registUser="register" @halamanLogin="changePage"></register-form-component>  
    <kanban-board-component v-else-if="currentPage == 'kanban page'" 
      :categories="categories"
      :tasks="tasks"
      @deleteId="deleteTask"
      @editTask="editTask"
      @addTask="createTask" 
    ></kanban-board-component>
  </div>
</template>

<script>

// import component
import NavbarComponent from './components/NavBar'
import RegisterFormComponent from './components/RegisterForm'
import LoginFormComponent from './components/LoginForm'
import axios from 'axios'
import swal from 'sweetalert';
import KanbanBoardComponent from './components/KanbanBoard'

const baseurl = "http://localhost:3001/"

export default {
  name : "App",
  data(){
    return {
      currentPage : "login page",
      categories : [
        {
          id : 1,
          name : 'backlog',
          color : 'bg-primary'
        },
        {
          id : 2,
          name : 'todo',
          color : 'bg-secondary'
        },
        {
          id : 3,
          name : 'doing',
          color : 'bg-warning'
        },{
          id : 4,
          name : 'done',
          color : 'bg-success'
        }
      ],
      tasks : []
    }
  },
  components :{
    NavbarComponent,
    LoginFormComponent,
    RegisterFormComponent,
    KanbanBoardComponent
  },
  methods:{
    changePage(page){
      this.currentPage = page
    },
    register(registUser){
      axios({
        url : `${baseurl}register`,
        method : "post",
        data :  registUser 
      })
      .then(response =>{
        swal({
          icon: "success",
          title: "Welcome",
          text : "registration success"
        })
        this.currentPage = 'login page'
      })
      .catch(err =>{
        // console.log(err.response.data);
        const errors = err.response.data.map(e=> e.message)
        swal({
          icon : "warning",
          title : "Error",
          text : errors.join(', ')
        })
      })
    },
    login(loginUser){
      axios({
          url : `${baseurl}login`,
          method : "post",
          data : loginUser
      })
      .then(response => {
        this.currentPage = 'kanban page'
        localStorage.setItem('access_token', token.data.access_token)
        this.fetch()
         swal({
        icon: "success",
        title: "Welcome",
        text : `${loginUser.email}`
      })
      })  
      .catch (error => {
       swal({
         icon: "error",
         title: "error",
         text : error.response.data.message
       })
      })
    },
    logout(page){
      swal({
        icon: "success",
        title: "logout",
        text : `byeeee`
      })
      localStorage.removeItem('access_token')
      var auth2 = gapi.auth2.getAuthInstance();
      auth2.signOut().then(function () {
        console.log('User signed out.');
      });

      this.currentPage = page;
    },
    fetch(){
      axios({
        url : `${baseurl}tasks`,
        method: 'get',
        headers :{
          access_token : localStorage.getItem('access_token')
        }
      })
      .then(response => {
        this.tasks = data.data
      }) 
      .catch ((error) => {
       // console.log(error);
       swal({
         icon: "error",
         title: "error",
         text : error.response.data.message
       })
      })
    },
    deleteTask(id){
      axios({
        url : `${baseurl}tasks/${id}`,
        method: 'delete',
        headers :{
          access_token : localStorage.getItem('access_token')
        }
      })
      .then( response => {
        this.fetch()
      })
      .catch (error => {
        swal({
          icon: "error",
          title: "error",
          text : error.response.data.message
        })
      })
    },
    editTask(data, id){
      axios ({
        url : `${baseurl}tasks/${id}`,
        method: 'put',
        headers :{
        access_token : localStorage.getItem('access_token')
        },
        data : data
      })
      .then(response => {
        this.fetch()
      })
      .catch (error => {
        this.fetch()
        swal({
          icon: "error",
          title: "error",
          text : error.response.data.message
        })
      })
    },
    createTask(data){
      axios({
          url : `${baseurl}tasks`,
          method: 'post',
          headers :{
          access_token : localStorage.getItem('access_token')
          },
          data : data
      })
      .then (response => {
        this.fetch()
      })
       .catch (error => {
        console.log(error);
        swal({
          icon: "error",
          title: "error",
          text : "failed"
        })
      })
    },
    async googleSignIn(token){
      // console.log(token);

      try {
        const googleLoginToken = await axios({
          url : `${baseurl}googleLogin`,
          data :{
            token
          },
          method : 'post'
        })
        // console.log(googleLoginToken.data.access_token);
        localStorage.setItem('access_token', googleLoginToken.data.access_token)
        this.currentPage = 'kanban page'
        this.fetch()
      } catch (error) {
        console.log(error);
      }
    }
        
  },
  created(){
    if(localStorage.getItem('access_token')){
      this.currentPage = 'kanban page'
      this.fetch()
    }else{
      this.currentPage = 'login page'
    }
  }  
}
</script>

<style>

</style>