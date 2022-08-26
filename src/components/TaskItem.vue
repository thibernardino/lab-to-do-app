<template>
  <div class="ctn">

    <div class="task-item" v-for="(task, index) in tasks" :key="index">
      <div class="task-image">
          <i class="task fas fa-solid fa-thumbtack"></i>
      </div>

      <div class="task-ctn">
        <div class="task-title">
          <h3 :class="task.is_complete ? 'done' : ''">{{ task.title }}</h3>
        </div>

        <div class="task-description">
          <p :class="task.is_complete ? 'done' : ''">{{ task.description }}</p>
        </div>

        <div class="task-buttons">
          <div :class="task.is_complete ? 'task-change-state' : 'task-change-state-not'" @click="setStatusTaskTask(task.id, index)"></div>

          <div class="task-change-name">
            <i class="change fas fa-edit fa-lg" @click="changeNameActiveValue(task.id)"></i>
          </div>

          <div class="task-delete">
            <i class="change fas fa-ban fa-lg" @click="deleteTask(task.id)"></i>
          </div>
        </div>

        <div class="changeName" v-if="changeNameActive && idRef === task.id">
          <div class="new-task-form">
            <div class="input-field">
              <input class="input-field-input" type="text" placeholder="Change title" v-model="name">
            </div>
            <button @click="changeNameTask(task.id, index)" class="button">Save</button>
          </div>
        </div>

      </div>
    </div>

  </div>
</template>

<script setup>
import { ref } from 'vue'

const emit = defineEmits([
  'statusTask', 
  'deleteTask', 
  'titleTask'
  ]);

const props = defineProps({
  tasks: Array,
});

const name = ref('');
const description = ref('');

//open the form to change the values insied
const changeNameActive = ref(false);
const idRef = ref(null);

const setStatusTaskTask = (id, index) => {

  //is_complete is a positive variable for then make the comparison
  const taskToToggle = props.tasks.map((task) =>
  task.id === id ? {...task, is_complete: !task.is_complete} : task
  );

  emit('statusTask', taskToToggle[index]);

};

const deleteTask = (id) => {

  const taskToDelete = props.tasks.filter((task) => task.id === id);
  emit('deleteTask', taskToDelete[0]);


};

const changeNameActiveValue = (id) => {
  idRef.value = id;
    changeNameActive.value = !changeNameActive.value;
};

const changeNameTask = (id, index) => {

  const taskToEdit = props.tasks.map((task) => 
  task.id === id ? {...task, title: name.value} : task
  );
  emit('titleTask', taskToEdit[index]);

  name.value = "";
  changeNameActive.value = false;

};

</script>

<style scoped>

.task-item{
  margin-top: 40px;
  height: 100%;
  position: relative;
  width: 33.333%;

}

.task-image{
  position: absolute;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background-color: #8f8d8d;
  left: 50%;
  top: -35px;
  transform: translate(-50%, 0);
  border: 7px solid #fff;
}

.ctn{
  max-width: 1100px;
  margin: auto;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
}

.task-ctn{
  padding: 30px;
  background-color: #fff;
  margin-left: 10px;
  margin-right: 10px;
  margin-bottom: 40px;
  color: #303134;
  border-radius: 50px;
}

.task-change-state{
  width: 40px;
  height: 40px;
  background-color: #8f8d8d;
}

.task-change-state-not{
  width: 40px;
  height: 40px;
  background-color: #393737;
}

.task-change-name{
  width: 40px;
  height: 40px;
  background-color: #8f8d8d;
}

.task-delete{
  width: 40px;
  height: 40px;
  background-color: #111111;
}

.task-buttons{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.task-title{
  margin-bottom: 30px;
}

.task-description{
  margin-bottom: 30px;
}



.task{
  color: #fff;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.task-change-state,
.task-change-state-not,
.task-change-name,
.task-delete{
  position: relative;
  border-radius: 6px;
  opacity: 0.5;
  cursor: pointer;
}

.task-change-state:hover,
.task-change-state-not:hover,
.task-change-name:hover,
.task-delete:hover{
  opacity: 1;
}

.change{
  color: #fff;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.changeName{
  margin-top: 20px;
}

.input-field-input{
  margin-bottom: 15px;
  width: 100%;
  padding-top: 10px;
  padding-bottom: 10px;
  border-radius: 5px;
}


.button{
  font-family: 'Roboto', sans-serif;
  width: 100%;
  padding: 10px;
  border: none;
  border-radius: 5px;
  background-color: #8f8d8d;
  color: #fff;
  font-weight: 700;
  font-size: 15px;
}

input[type="text" i]{
  border: 0;
  padding-left: 0;
  padding-right: 0;
  box-shadow: 0 1px 8px rgba(48, 49, 52, 0.3);;
}

.button:hover {
  color: rgba(255, 255, 255, 1) !important;
  box-shadow: 0 2px 8px #000000;
  transition: all 0.2s ease;
}

.done{
  text-decoration:line-through;
}

.no-task-text{
  color: #303134;
  font-size: 25px;
}

@media only screen and (max-width: 747px){
  .ctn{
    margin-left: 30px;
    margin-right: 30px;

  }

  .task-item{
    width: 50%;
    margin-left: 0px;
    margin-right: 0px;
  }
}

</style>