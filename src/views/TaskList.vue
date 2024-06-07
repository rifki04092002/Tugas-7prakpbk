<!-- src/views/TaskList.vue -->
<template>
  <div class="task-list-container">
    <h1>Daftar Tugas</h1>
    <input
      v-model="newTask"
      @keyup.enter="addTask"
      placeholder="Tambah tugas baru"
      class="task-input"
    />
    <button @click="addTask" class="add-button">Tambah</button>
    <ul class="task-list">
      <li v-for="task in tasks" :key="task.id" class="task-item">
        <input
          type="checkbox"
          :checked="task.completed"
          @change="toggleTaskCompletion(task.id)"
          class="task-checkbox"
        />
        <span
          :class="{ completed: task.completed, 'task-text': true }"
          @click="toggleTaskCompletion(task.id)"
        >
          {{ task.text }}
        </span>
        <button @click="removeTask(task.id)" class="delete-button">
          Hapus
        </button>
      </li>
    </ul>
    <p class="incomplete-tasks-count">
      Jumlah tugas yang belum selesai: {{ incompleteTasksCount }}
    </p>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import { useTaskStore } from "@/stores/taskStore";

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

    const incompleteTasksCount = computed(() => taskStore.incompleteTasksCount);

    return {
      newTask,
      tasks: taskStore.tasks,
      incompleteTasksCount,
      addTask,
      removeTask,
      toggleTaskCompletion,
    };
  },
};
</script>

<style>
.task-list-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 10px;
  background-color: #f9f9f9;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  color: #333;
}

.input-container {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.task-input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  margin-right: 10px;
}

.add-button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  background-color: #28a745;
  color: #fff;
  cursor: pointer;
}

.add-button:hover {
  background-color: #218838;
}

.task-list {
  list-style: none;
  padding: 0;
}

.task-item {
  display: flex;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

.task-item:last-child {
  border-bottom: none;
}

.task-checkbox {
  margin-right: 10px;
}

.task-text {
  flex: 1;
  cursor: pointer;
}

.task-text.completed {
  text-decoration: line-through;
  color: #888;
}

.delete-button {
  padding: 5px 10px;
  border: none;
  border-radius: 5px;
  background-color: #dc3545;
  color: #fff;
  cursor: pointer;
}

.delete-button:hover {
  background-color: #c82333;
}

.incomplete-tasks-count {
  text-align: center;
  margin-top: 20px;
  color: #555;
}

.todo-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #f90909;
  border-radius: 10px;
  background-color: #07fc0f;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
</style>
