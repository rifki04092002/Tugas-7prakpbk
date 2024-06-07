<!-- src/components/TaskList.vue -->
<template>
  <div>
    <h1>Daftar Tugas</h1>
    <input
      v-model="newTask"
      @keyup.enter="addTask"
      placeholder="Tambah tugas baru"
    />
    <button @click="addTask">Tambah</button>
    <ul>
      <li v-for="task in tasks" :key="task.id">
        <span
          :class="{ completed: task.completed }"
          @click="toggleTaskCompletion(task.id)"
        >
          {{ task.text }}
        </span>
        <button @click="removeTask(task.id)">Hapus</button>
      </li>
    </ul>
    <p>Jumlah tugas yang belum selesai: {{ incompleteTasksCount }}</p>
  </div>
</template>

<script>
import { ref } from "vue";
import { useCounstStore } from "./stores/CountStores";

export default {
  setup() {
    const taskStore = useTaskStore();
    const newTask = ref("");

    const addTask = () => {
      if (newTask.value.trim()) {
        taskStore.addTask(newTask.value);
        newTask.value = "";
      }
    };

    const removeTask = (taskId) => {
      taskStore.removeTask(taskId);
    };

    const toggleTaskCompletion = (taskId) => {
      taskStore.toggleTaskCompletion(taskId);
    };

    return {
      newTask,
      tasks: taskStore.tasks,
      incompleteTasksCount: taskStore.incompleteTasksCount,
      addTask,
      removeTask,
      toggleTaskCompletion,
    };
  },
};
</script>

<style>
.completed {
  text-decoration: line-through;
}
</style>
