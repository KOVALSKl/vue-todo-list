<script>
import TaskVue from "./Task.vue";
import { ref, watch, onMounted } from "vue";
export default {
  components: {
    TaskVue: TaskVue,
  },
  setup() {
    let tasks = ref([]);

    let sortedTasks = ref([]);

    // вместо этой штуки лучше было бы использовать enum, если бы я конечно же
    // использовал typescript
    let sortBy = ref("all");

    onMounted(() => {
      if (localStorage.tasks) {
        tasks.value = JSON.parse(localStorage.tasks);
      }
    });

    watch(sortBy, (sortType) => {
      sortedTasks.value = sortTasks(tasks.value, sortType);
    });

    watch(
      tasks,
      (newValue) => {
        localStorage.setItem(
          "tasks",
          JSON.stringify([
            ...newValue.map((item) => JSON.parse(JSON.stringify(item))),
          ])
        );
        sortedTasks.value = sortTasks([...newValue], sortBy.value);
      },
      {
        deep: true,
      }
    );

    function sortTasks(tasks, sortType) {
      switch (sortType) {
        case "all": {
          return tasks;
        }
        case "finished": {
          return tasks.filter((item) => item.finished);
        }
        case "active": {
          return tasks.filter((item) => !item.finished);
        }
      }
    }

    function onTaskUpdate({ id, newValue }) {
      tasks.value.filter((item) => {
        if (item.id === id) {
          item.title = newValue.value.title;
          item.date = newValue.value.date;
          item.body = newValue.value.body;
          item.finished = newValue.value.finished;
        }
      });
    }

    function onTaskStatusUpdate({ id, status }) {
      tasks.value.filter((item) => {
        if (item.id === id) {
          item.finished = status;
        }
      });
    }

    function onTaskDelete(id) {
      tasks.value = tasks.value.filter((item) => item.id != id);
    }

    function addNewTask() {
      sortBy.value = "all";
      tasks.value.unshift({
        id: new Date().getTime(),
        title: "",
        body: "",
        date: new Date().toISOString().slice(0, 10),
        finished: false,
      });
    }

    return {
      onTaskUpdate,
      onTaskStatusUpdate,
      onTaskDelete,
      addNewTask,
      tasks,
      sortBy,
      sortedTasks,
    };
  },
};
</script>

<template>
  <div class="tasks-sort-status">
    <label for="sort-by-status-select">Сортировка задач по статусу:</label>
    <select class="sort-by-list" v-model="sortBy" id="sort-by-status-select">
      <option value="all">все</option>
      <option value="finished">выполненые</option>
      <option value="active">активные</option>
    </select>
  </div>
  <ul class="tasks-list" v-if="sortedTasks.length > 0">
    <li
      class="task-list__item-container"
      v-for="task in sortedTasks"
      :key="task.id"
    >
      <TaskVue
        :task="{ task }"
        @taskDelete="onTaskDelete"
        @taskUpdate="onTaskUpdate"
        @taskStatusUpdate="onTaskStatusUpdate"
      />
    </li>
  </ul>
  <ul class="tasks-list" v-else>
    <li>There are no tasks here yet</li>
  </ul>
  <div class="options-button">
    <button class="add-new-task" @click="addNewTask">Add</button>
  </div>
</template>

<style>
.tasks-sort-status {
  display: flex;
  align-items: center;
}

ul.tasks-list {
  display: flex;
  flex-direction: column;
  align-items: center;
  flex-wrap: wrap;
  gap: 20px;

  max-height: 600px;
  width: 1000px;

  border: 1px solid lightgray;

  overflow: hidden;
  overflow-x: scroll;

  list-style-type: none;
  padding: 30px;
}
</style>