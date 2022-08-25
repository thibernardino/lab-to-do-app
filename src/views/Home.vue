<template>
  <Nav />

  <div class="bg">
    <NewTask @new-task="brandNewTask" />
    <!-- <NewTask /> -->
    <TaskItem
      :tasks="editNewTask.tasks"
      @statusTask="setStatusTask"
      @deleteTask="eraseTask"
      @titleTask="editTitleTask"
    />
  </div>

  <Footer />
</template>

<script setup>
import { ref } from "vue";
import { useTaskStore } from "../stores/task";
import Nav from "../components/Nav.vue";
import NewTask from "../components/NewTask.vue";
import TaskItem from "../components/TaskItem.vue";
import Footer from "../components/Footer.vue";

const editNewTask = useTaskStore();

editNewTask.fetchTasks();
//add task
async function brandNewTask(task) {
  await editNewTask.addTask(task.name, task.description);
  editNewTask.fetchTasks();
}
//done - not done
async function setStatusTask(task) {
  await editNewTask.toggleTask(task.is_complete, task.id);
  editNewTask.fetchTasks();
}
//delete task
async function eraseTask(task) {
  await editNewTask.deleteTask(task.id);
  editNewTask.fetchTasks();
}
//edit task
async function editTitleTask(task) {
  await editNewTask.editTask(task.title, task.id);
  editNewTask.fetchTasks();
}
</script>

<style scoped>
.bg {
  background-color: #272525;
}
</style>
