<template>
<div>
  <div class="card-container mt-2" >
    <div v-for="task in filterTask" :key="task.id">
     <kanban-card-component 
      :filterTask="task" 
      :currentEditId="currentEditId"
      :editTogleStatus="editTogleStatus"
      @deleteId="deleteTask" 
      @editTask="edit"
    ></kanban-card-component>
    </div>
     
  </div> 

</div>
</template>

<script>
import kanbanCardComponent from './KanbanCard'
export default {
  components: { kanbanCardComponent },
  name : 'TaskBoard',
  props : ['tasks','category','currentEditId','editTogleStatus'],
  computed : {
    filterTask(){
      return this.tasks.filter(e=> e.category == this.category)
    }
  },
  methods: {
    deleteTask(id){
      this.$emit('deleteId', id)
    },
    edit(data,id){
      console.log(data,id, 'di task board');
      this.$emit('editTask', data, id)
    }
  }
}
</script>

<style>

</style>