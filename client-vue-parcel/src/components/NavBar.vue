<template>
  <nav class="navbar navbar-dark bg-dark justify-content-between">
    <a class="navbar-brand" href="/">KanbanBoard</a>
    <button v-if="currentPage != 'login page' && currentPage != 'register page'" @click.prevent="logout" class="btn btn-outline-primary">logout</button>
  </nav>
</template>

<script>



export default {
  name : "NavBar",
  props : ['currentPage'],
  methods :{
    logout(page){
      this.$emit('halamanLogin', page)
    },
    logout(){
      // console.log('a');
      this.$emit("halamanLogin","loginPage")
    }
  }
}
</script>

<style></style>
