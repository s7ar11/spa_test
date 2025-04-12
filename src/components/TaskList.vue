<template>
  <div class="task-list">
    <h2>Список задач</h2>
    <div v-for="task in tasks" :key="task.id" class="task-item">
      <input type="checkbox" v-model="task.completed" @change="saveTasks" />
      <span :class="{ completed: task.completed }">{{ task.title }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "TaskList",
  data() {
    return {
      tasks: [],
    };
  },
  methods: {
    async fetchTasks() {
      try {
        const response = await fetch("/tasks.json");
        const defaultTasks = await response.json();
        const savedTasks = localStorage.getItem("tasks");
        this.tasks = savedTasks ? JSON.parse(savedTasks) : defaultTasks;
      } catch (error) {
        console.error("Ошибка загрузки задач:", error);
      }
    },
    saveTasks() {
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
  },
  created() {
    this.fetchTasks();
  },
};
</script>

<style>
.task-list {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}
.task-item {
  padding: 10px;
  border-bottom: 1px solid #eee;
  display: flex;
  align-items: center;
}
.completed {
  text-decoration: line-through;
  color: #888;
}
input[type="checkbox"] {
  margin-right: 10px;
}
</style>
