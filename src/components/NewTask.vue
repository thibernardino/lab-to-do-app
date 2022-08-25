<template>
  <div class="new-task-section">
    <div class="new-task-title">
      <h1>Add a new Task</h1>
    </div>

    <div class="new-task-description">
      <p class="new-task-text">For every minute spent organizing, an hour is earned.</p>
      <p class="new-task-text">Today's Date is {{ date }}</p>
    </div>


    <div v-if="showErrorMessage">
      <p class="error-text">{{ errorMessage }}</p>
    </div>

    <div class="new-task-form">
      <div class="input-field">
        <input class="input-field-input" type="text" placeholder="Task Title" v-model="name">
      </div>

      <div class="input-field">
        <input class="input-field-input" type="text" placeholder="Task Description" v-model="description">
      </div>
      
      <button @click="errorFunction" class="button">Add</button>
    </div>

  </div>
</template>

<script setup>
import { ref } from "vue";
import { useTaskStore } from "../stores/task" 

import moment from 'moment'

const date = moment().format('ll');;   

// constant to save a variable that define the custom event that will be emitted to the homeView
const setTask = useTaskStore();

// constant to save a variable that holds the value of the title input field of the new task
const name = ref('');

// constant to save a variable that holds the value of the description input field of the new task
const description = ref('');

// constant to save a variable that holds an initial false boolean value for the errorMessage container that is 

const showErrorMessage = ref(false);

// const constant to save a variable that holds the value of the error message
const errorMessage = ref(null);

const emit = defineEmits(['new-task']);

const errorFunction = () => {
  if(name.value.length === 0 || description.value.length === 0){
    showErrorMessage.value = true;
    errorMessage.value = 'The task title or description is empty';
    setTimeout(() => {
      // errorMessage.value = null;
      showErrorMessage.value = false;
    }, 5000);
  } else {
    const newTask = {
      name: name.value,
      description: description.value,
    };

    emit("new-task", newTask);
    name.value = '';
    description.value = '';
  }
};

</script>

<style scoped>

.new-task-section{
  margin: auto;
  max-width: 1100px;
}

.new-task-section{
  text-align: center;
}

.new-task-title h1{
  font-size: 60px;
  margin-top: 0;
  padding-top: 30px;
  color: #8f8d8d;
}

.new-task-text{
  font-weight: 700;
  color: #8f8d8d;
}

.new-task-text:last-child{
  margin-bottom: 40px;
}

.input-field-input{
  margin-bottom: 15px;
  width: 100%;
  padding-top: 10px;
  padding-bottom: 10px;
  border-radius: 5px;
}

.new-task-form{
  margin-left: 30px;
  margin-right: 30px;
}

.button{
  font-family: 'Roboto', sans-serif;
  width: 100%;
  padding: 10px;
  border: none;
  border-radius: 5px;
  background-color: #8f8d8d;
  margin-bottom: 40px;
  color: #fff;
  font-weight: 700;
  font-size: 15px;
  cursor:pointer;
}

input[type="text" i]{
  border: 0;
  padding-left: 0;
  padding-right: 0;
}

.button:hover {
  color: rgba(255, 255, 255, 1) !important;
  box-shadow: 0 2px 8px #000000;
  transition: all 0.2s ease;
}

.error-text{
  color: #393737;
}


</style>
