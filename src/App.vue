<script>
import TaskListVue from "./components/TaskList.vue";

export default {
  name: "App",
  components: {
    TaskListVue: TaskListVue,
  },
};
</script>

// <script setup>
// import { ref, watch, onMounted } from "vue";

// let tasks = ref([]);

// onMounted(() => {
//   if (localStorage.tasks) {
//     tasks.value = JSON.parse(localStorage.tasks);
//   }
// });

// watch(
//   tasks,
//   (newValue) => {
//     localStorage.setItem(
//       "tasks",
//       JSON.stringify([
//         ...newValue.map((item) => JSON.parse(JSON.stringify(item))),
//       ])
//     );
//   },
//   {
//     deep: true,
//   }
// );

// function onTaskUpdate({ id, newValue }) {
//   tasks.value.filter((item) => {
//     if (item.id === id) {
//       item.title = newValue.value.title;
//       item.date = newValue.value.date;
//       item.body = newValue.value.body;
//       item.finished = newValue.value.finished;
//     }
//   });
// }

// function onTaskStatusUpdate({ id, status }) {
//   tasks.value.filter((item) => {
//     if (item.id === id) {
//       item.finished = status;
//     }
//   });
// }

// function onTaskDelete(id) {
//   tasks.value = tasks.value.filter((item) => item.id != id);
// }

// function addNewTask() {
//   tasks.value.push({
//     id: new Date().getTime(),
//     title: "",
//     body: "",
//     date: new Date().toISOString().slice(0, 10),
//     finished: false,
//   });
// }
//
</script>


<template>
  <TaskListVue />
</template>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;

  height: 100%;
  width: 100%;

  display: flex;
  justify-content: center;
  align-items: center;

  flex-direction: column;
}
</style>
