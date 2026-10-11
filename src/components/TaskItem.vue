<script setup>
const props = defineProps({
  task: {
    type: Object,
    required: true,
  },
});

const emit = defineEmits(["complete", "delete"]);

function completeTask() {
  emit("complete", props.task.id);
}

function deleteTask() {
  emit("delete", props.task.id);
}
</script>

<template>
  <li
    class="task-item"
    :class="{
      completed: task.completed,
      highPriority: task.priority === 'High',
    }"
  >
    <div class="task-info">
      <h2>{{ task.title }}</h2>

      <p>
        Priority:
        <strong>{{ task.priority }}</strong>
      </p>

      <p>
        Status:
        <strong>
          {{ task.completed ? "Completed" : "Active" }}
        </strong>
      </p>
    </div>

    <div class="task-actions">
      <button
        v-if="!task.completed"
        type="button"
        class="complete-button"
        @click="completeTask"
      >
        Complete
      </button>

      <button type="button" class="delete-button" @click="deleteTask">
        Delete
      </button>
    </div>
  </li>
</template>

<style scoped>
.task-item {
  margin-bottom: 15px;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  background: #fafafa;
  transition: 0.2s;
}

.task-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
}

.task-item h2 {
  margin: 0 0 10px;
  color: #222;
  font-size: 20px;
}

.task-item p {
  margin: 6px 0;
  color: #555;
}

.completed {
  opacity: 0.6;
}

.completed h2 {
  text-decoration: line-through;
}

.highPriority {
  border: 2px solid #d9534f;
}

.task-actions {
  display: flex;
  gap: 10px;
  margin-top: 15px;
}

.task-actions button {
  padding: 8px 14px;
  border: none;
  border-radius: 6px;
  color: white;
  cursor: pointer;
}

.complete-button {
  background: #198754;
}

.complete-button:hover {
  background: #146c43;
}

.delete-button {
  background: #dc3545;
}

.delete-button:hover {
  background: #b02a37;
}
</style>
