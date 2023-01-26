<script setup>
import { defineProps, defineEmits, ref, computed, watch } from "vue";
const props = defineProps(["task"]);
const emit = defineEmits(["taskDelete", "taskUpdate", "taskStatusUpdate"]);

const taskData = ref({
  title: props.task.task.title,
  date: props.task.task.date,
  body: props.task.task.body,
  finished: props.task.task.finished,
});

let isEditing = ref(true);

const editButtonText = computed(() => {
  return isEditing.value ? "Save" : "Edit";
});

// НАБЛЮДАТЕЛИ

watch(
  taskData,
  (newValue) => {
    taskData.value = newValue;
  },
  { deep: true }
);

// ВСПОМОГАТЕЛЬНЫЕ ФУНКЦИИ

function setIsEditing() {
  if (checkTaskFields()) isEditing.value = !isEditing.value;
}

function checkTaskFields() {
  return taskData.value.title && taskData.value.date && taskData.value.body;
}

// EMIT'ы событий

function emitTaskUpdateEvent() {
  if (!isEditing.value)
    emit("taskUpdate", { id: props.task.task.id, newValue: taskData });
}

function emitTaskDeleteEvent() {
  emit("taskDelete", props.task.task.id);
}

function emitTaskStatusUpdateEvent() {
  emit("taskStatusUpdate", {
    id: props.task.task.id,
    status: taskData.value.finished,
  });
}
</script>

<template>
  <div class="task-item">
    <form @submit.prevent @submit="emitTaskUpdateEvent">
      <div class="task-item-head">
        <div class="task-item__header">
          <input
            class="title"
            placeholder="Task title"
            :class="{ editing: isEditing }"
            :disabled="!isEditing"
            v-model="taskData.title"
            required
          />
          <input
            class="task-status"
            type="checkbox"
            name="status"
            v-model="taskData.finished"
            @change="emitTaskStatusUpdateEvent"
          />
        </div>
        <input
          class="task-creation__time"
          type="date"
          :class="{ editing: isEditing }"
          :disabled="!isEditing"
          v-model="taskData.date"
          required
        />
      </div>
      <div class="task-item__body">
        <textarea
          placeholder="Task body"
          :class="{ editing: isEditing }"
          :disabled="!isEditing"
          v-model="taskData.body"
          required
        />
      </div>
      <div class="options-buttons">
        <button @click="setIsEditing" type="submit">
          {{ editButtonText }}
        </button>
        <button @click="emitTaskDeleteEvent">Delete</button>
      </div>
    </form>
  </div>
</template>

<style>
.task-item {
  width: 250px;

  display: flex;
  flex-direction: column;
  gap: 10px;

  border: 1px solid rgba(0, 0, 0, 0.5);
  border-radius: 10px;
  padding: 10px;

  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.2);

  transition: box-shadow 450ms ease-in-out, scale 450ms ease-in-out;
}

.task-item input,
.task-item textarea,
.task-item input:disabled,
.task-item textarea:disabled {
  outline: none;
  border: 0.5px solid transparent;
  border-radius: 5px;
  box-sizing: border-box;

  max-width: 100%;

  transition: border 350ms ease-in-out;

  font-family: Arial, Helvetica, sans-serif;

  background-color: transparent;
  color: #000000;

  overflow: hidden;
  overflow-x: scroll;
  overflow-y: scroll;
}

.task-item input[type="date"] {
  width: fit-content;
}

.task-item textarea {
  min-width: 100%;

  min-height: 50px;
  max-height: 80px;
}

.task-item input.editing:hover,
.task-item textarea.editing:hover {
  border: 0.5px solid rgba(0, 0, 0, 0.2);
}

.task-item input.editing:focus,
.task-item textarea.editing:focus {
  border: 0.7px solid rgba(0, 0, 0, 0.5);
}

.task-item:hover {
  box-shadow: 0px 8px 10px rgba(0, 0, 0, 0.2);
  scale: 1.05;
}

.task-item .task-item-head {
  width: 100%;

  display: flex;
  flex-direction: column;
  gap: 3px;
}

.task-item .task-item-head .task-item__header {
  display: flex;
  justify-content: space-between;
}

.task-item .task-item-head .title {
  font-size: 20px;
}

.task-item .options-buttons {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
}
</style>