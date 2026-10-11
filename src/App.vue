<script setup>
import { computed, ref } from "vue";
import TaskItem from "./components/TaskItem.vue";

const tasks = ref([
  {
    id: 1,
    title: "Learn Vue basics",
    completed: true,
    priority: "High",
  },
  {
    id: 2,
    title: "Practice Vue directives",
    completed: false,
    priority: "Medium",
  },
  {
    id: 3,
    title: "Create To Do App",
    completed: false,
    priority: "Low",
  },
]);

const newTask = ref("");
const newPriority = ref("Medium");

const statusFilter = ref("All");
const priorityFilter = ref("All");

const filteredTasks = computed(function () {
  return tasks.value.filter(function (task) {
    const matchesStatus =
      statusFilter.value === "All" ||
      (statusFilter.value === "Completed" && task.completed) ||
      (statusFilter.value === "Active" && !task.completed);

    const matchesPriority =
      priorityFilter.value === "All" || task.priority === priorityFilter.value;

    return matchesStatus && matchesPriority;
  });
});

function addTask() {
  if (newTask.value.trim() === "") {
    return;
  }

  const task = {
    id: Date.now(),
    title: newTask.value.trim(),
    completed: false,
    priority: newPriority.value,
  };

  tasks.value.push(task);

  newTask.value = "";
  newPriority.value = "Medium";
}

function completeTask(id) {
  const task = tasks.value.find(function (task) {
    return task.id === id;
  });

  if (task) {
    task.completed = true;
  }
}

function deleteTask(id) {
  tasks.value = tasks.value.filter(function (task) {
    return task.id !== id;
  });
}

function clearFilters() {
  statusFilter.value = "All";
  priorityFilter.value = "All";
}
</script>

<template>
  <main class="todo-app">
    <h1>To Do App</h1>

    <div class="task-form">
      <input
        v-model="newTask"
        type="text"
        placeholder="Enter a new task"
        @keyup.enter="addTask"
      />

      <select v-model="newPriority">
        <option value="High">High</option>
        <option value="Medium">Medium</option>
        <option value="Low">Low</option>
      </select>

      <button type="button" @click="addTask">Add Task</button>
    </div>

    <div class="filters">
      <div class="filter-group">
        <label for="statusFilter">Status</label>

        <select id="statusFilter" v-model="statusFilter">
          <option value="All">All</option>
          <option value="Active">Active</option>
          <option value="Completed">Completed</option>
        </select>
      </div>

      <div class="filter-group">
        <label for="priorityFilter">Priority</label>

        <select id="priorityFilter" v-model="priorityFilter">
          <option value="All">All</option>
          <option value="High">High</option>
          <option value="Medium">Medium</option>
          <option value="Low">Low</option>
        </select>
      </div>

      <button
        v-show="statusFilter !== 'All' || priorityFilter !== 'All'"
        type="button"
        class="clear-button"
        @click="clearFilters"
      >
        Clear Filters
      </button>
    </div>

    <p class="task-count">
      Showing {{ filteredTasks.length }} of {{ tasks.length }} tasks
    </p>

    <p v-if="tasks.length === 0" class="empty-message">No tasks available</p>

    <p v-else-if="filteredTasks.length === 0" class="empty-message">
      No tasks match the selected filters
    </p>

    <ul v-else>
      <TaskItem
        v-for="task in filteredTasks"
        :key="task.id"
        :task="task"
        @complete="completeTask"
        @delete="deleteTask"
      />
    </ul>
  </main>
</template>

<style scoped>
.todo-app {
  width: 700px;
  max-width: calc(100% - 30px);
  margin: 60px auto;
  padding: 30px;
  background: #ffffff;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
}

h1 {
  margin-top: 0;
  margin-bottom: 30px;
  text-align: center;
  color: #222;
}

.task-form {
  display: flex;
  gap: 10px;
  margin-bottom: 25px;
}

.task-form input {
  flex: 1;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 15px;
}

.task-form select {
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 6px;
  background: white;
  font-size: 15px;
}

.task-form button {
  padding: 12px 18px;
  border: none;
  border-radius: 6px;
  background: #222;
  color: white;
  cursor: pointer;
}

.task-form button:hover {
  background: #444;
}

.filters {
  display: flex;
  align-items: flex-end;
  gap: 12px;
  margin-bottom: 15px;
  padding: 18px;
  background: #f5f5f5;
  border-radius: 8px;
}

.filter-group {
  flex: 1;
}

.filter-group label {
  display: block;
  margin-bottom: 6px;
  color: #555;
  font-size: 13px;
}

.filter-group select {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 6px;
  background: white;
}

.clear-button {
  padding: 10px 14px;
  border: 1px solid #222;
  border-radius: 6px;
  background: white;
  color: #222;
  cursor: pointer;
}

.clear-button:hover {
  background: #222;
  color: white;
}

.task-count {
  margin: 0 0 20px;
  color: #777;
  font-size: 14px;
  text-align: right;
}

ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

.empty-message {
  padding: 30px;
  text-align: center;
  color: #777;
  border: 1px dashed #ccc;
  border-radius: 8px;
}

@media (max-width: 600px) {
  .todo-app {
    margin: 25px auto;
    padding: 20px;
  }

  .task-form {
    flex-direction: column;
  }

  .task-form input,
  .task-form select,
  .task-form button {
    width: 100%;
  }

  .filters {
    flex-direction: column;
    align-items: stretch;
  }

  .clear-button {
    width: 100%;
  }
}
</style>
