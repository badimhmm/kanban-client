<template>
  <div>
    <div class="conBoards__board__card">
        <div class="card-body mt-3 shadow-sm">
        <h5 class="card-title">{{ filterTask.title }}</h5>
        <small>author</small>
        <p class="card-text">{{filterTask.User.email.split('@')[0]}}</p>
        <div class="card-footer">
          <button @click.prevent="toggleEdit(filterTask)" type="button" class="btn btn-outline-primary far fa-edit"></button>
          <button @click.prevent="deleteTask(filterTask.id)" type="button" class="btn btn-outline-primary far fa-trash-alt"></button>
        </div>
      </div>
    </div>
    <div v-if="editToggle && (currentEditId == 'none' || currentEditId == editData.id)">
      <form @submit.prevent="edit" action="">
        <textarea v-model="editTitle" name="" id="" cols="22" rows="2" required></textarea>
        <!-- <button @click.prevent="toggleEdit(filterTask)" type="button" class="btn btn-outline-primary fas fa-times"></button> -->
        <select v-model="editCategory" name="" id="">
          <option value="backlog">Backlog</option>
          <option value="todo">Todo</option>
          <option value="doing">Doing</option>
          <option value="done">Done</option>
        </select>
        <button  type="submit" class="btn btn-outline-primary fas fa-check"></button>
      </form>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style></style>
