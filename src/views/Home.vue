<template>
  <div class="wraper">
    <div class="main-page">
      <Nav />

      <div class="content welcome">
        <div class="wellcome-body">
          <h3>Start <span>tracking</span> your tasks!</h3>

          <lottie-player
            class="chicoslogin"
            src="https://lottie.host/51579329-b609-4c3d-b5e6-9bde1eeceaa4/aGYMKXWte7.json"
            background="transparent"
            speed="1"
            style="width: 200px; height: 200px"
            loop
            autoplay
          ></lottie-player>
        </div>
        <!-- <div>
        <transition name="fade">
          <div v-if="showP" class="cuadrado"></div>
        </transition>
        <button @click="showP = !showP">toggle</button>
      </div> -->
      </div>
      <section class="min-height">
        <NewTask @getTasks="getTasks" />
        <div class="tareas-flex">
          <TaskItem
            @deleteTask="deleteTask"
            @toogleTask="toogleTask"
            @getTasks="getTasks"
            v-for="task in tasks"
            :key="task.id"
            :task="task"
          />
        </div>
      </section>
    </div>
    <FooterVue />
  </div>
</template>
<!-- creamos un task item por cada tarea que tengamos en el array tareas. -->

<script setup>
import { ref } from "vue";
import { useUserStore } from "../stores/user";
import { useTaskStore } from "../stores/task";
import { useRouter } from "vue-router";
import Nav from "../components/Nav.vue";
import NewTask from "../components/NewTask.vue";
import TaskItem from "../components/TaskItem.vue";
import FooterVue from "../components/footer.vue";

const showP = ref(false);
// const getUser = computed(() => useUserStore().user);
const getUser = useUserStore().user;

// constant that calls user email from the useUSerStore
const userEmail = getUser.email;

const taskStore = useTaskStore();

// Variable para guardar las tareas de supabase
const tasks = ref([]);

// Creamos una función que conecte a la store para conseguir las tareas de supabase
const getTasks = async () => {
  tasks.value = await taskStore.fetchTasks();
};

getTasks();

const loadComponent = () => {
  setTimeout(() => {
    showP.value = true;
  }, 1000);
  console.log("hola");
};
loadComponent();

const deleteTask = async () => {
  tasks.value = await taskStore.fetchTasks();
};

deleteTask();

// toogle
const toogleTask = async () => {
  tasks.value = await taskStore.fetchTasks();
};

toogleTask();
</script>

<style>
.fade-enter-from {
  opacity: 0;
}
.fade-enter-to {
  opacity: 1;
}
.fade-enter-active {
  transition: all 3s ease;
}
.fade-leave-from {
  opacity: 1;
}
.fade-leave-to {
  opacity: 0;
}
.fade-leave-active {
  transition: all 3s ease;
}
</style>

<!-- 
**Hints**
1. ref() is used here!
2. (NewTask, TaskItem, Footer, Nav) components are used here! 
3. Tasks are going to be contained in an array here!
4. An async function is needed to get all of the tasks stored within the supabase database, this async function's body will 
contain the tasks value which be use to store the fetchTasks method which lives inside the userTaskStore. This function 
needs to be called within the setUp script in order to run within the first instance of this component lifecycle.

5. NewTask component will receive a customEvent on this instance of the homeView that will fire the add-to-do function
6. add-to-do function will receive 2 params/arguments that will tak a taskTitle and a taskDescription and the body of this 
async function will call the taskStore that calls the addTask function from the store that pushes the info of the task to the 
backEnd. This is possible by passing the 2 param/arguments that will be passed by the user from the inputs within the NewTask 
Component. 

7. TaskItem component will loop through the tasks-array that will print an individual instance of an individual TaskItem 
component. TaskItem will receive 3 customEvents on this instance of the homeView. 1 customEvent for toggling the task to show 
either a text or an icon to display if the task is completed or not completed. 1 customEevent for removing/deleting the 
task out of the array. 1 customEvent for editing the task title and description. This function needs to call the function 
mentioned on hint4.


7.1-customEvent will fire an async function that will take in 1 param/argument. On the body of this function the param/argument 
will be used to define 2 constants. 1 of this constants will take care of setting the boolean value to the opposite of the 
value that checks wether this task is_complete. 1 of this constants will take of calling the id of this specific task in 
order to call the right id. 
7.2-customEvent will fire an asynf function that will take in 1 param/argument. This async function's body will be used to 
call the deleteTaskmethod which will take the param/argument's id in order to delete the task. This function needs to call 
the function mentioned on hint4. 
7.3-customEvent will fire an async function that will take in 1 param/argument. this async function's body will be used to 
take in 2 constants. 1 constant will take in the param/argument newValue. 1 constant will be used to get the param/argument 
oldValue id. These 2 constants will be sent to the backend via the useTaskStore which holds an editTask method. This function 
needs to call the function mentioned on hint4.
-->
