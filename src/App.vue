<script>
import TaskListVue from "./components/TaskList.vue";

export default {
  name: "App",
  components: {
    TaskListVue: TaskListVue,
  },
};
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
