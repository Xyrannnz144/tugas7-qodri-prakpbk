<template>
  <div class="todo-list">
    <h1>Todo List</h1>
    <br />
    <br />
    <div class="input-group">
      <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Add a New Task" />
      <button @click="addTodo" class="add-button">Add</button>
    </div>
    <ul>
      <li v-for="(todo, index) in todos" :key="index" :class="{ done: todo.done }">
        <span>{{ todo.text }}</span>
        <div class="actions">
          <button @click="toggleTodo(index)" class="complete-button">
            {{ todo.done ? 'Mark as Undone' : 'Mark as Done' }}
          </button>
          <button @click="confirmDelete(index)" class="delete-button">Delete</button>
        </div>
      </li>
    </ul>
    <br>
    <p>Unfinished Task : {{ unfinishedTodos }}</p>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';
import { useTodoStore } from '../stores/todo.js';

const store = useTodoStore();
const newTodo = ref('');

const addTodo = () => {
  if (newTodo.value.trim() !== '') {
    store.addTodo(newTodo.value);
    newTodo.value = '';
  }
};

const removeTodo = (index) => {
  store.removeTodo(index);
};

const toggleTodo = (index) => {
  store.toggleTodo(index);
};

const confirmDelete = (index) => {
  if (confirm("Are you sure you want to delete this task?")) {
    removeTodo(index);
    alert("Task deleted successfully!");
  }
};

const todos = store.todos;
const unfinishedTodos = ref(store.unfinishedTodos);

watch(
  () => store.unfinishedTodos,
  (newCount) => {
    unfinishedTodos.value = newCount;
  }
);
</script>

<style scoped>
.todo-list {
  max-width: 500px;
  margin: 40px auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  background-color: #f7f7f7;
}

.todo-list h1 {
  font-size: 24px;
  font-weight: bold;
  color: #333;
  margin-bottom: 10px;
}

.input-group {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.todo-list input {
  width: calc(100% - 80px);
  padding: 12px;
  margin-right: 10px;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 16px;
}

.add-button {
  padding: 12px;
  background-color: #2ecc71; /* New unique color */
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

.add-button:hover {
  background-color: #27ae60; /* Darker shade for hover */
}

.todo-list ul {
  list-style: none;
  padding: 0;
}

.todo-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px;
  border-bottom: 1px solid #ddd;
}

.todo-list li.done span {
  text-decoration: line-through;
  color: #999;
}

.actions {
  display: flex;
  gap: 10px;
}

.complete-button {
  background-color: #3dbb95; /* Blue for complete button */
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 6px;
  cursor: pointer;
}

.complete-button:hover {
  background-color: #0d1013; /* Darker shade for hover */
}

.delete-button {
  background-color: #e74c3c; /* Red for delete button */
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 6px;
  cursor: pointer;
}

.delete-button:hover {
  background-color: #c0392b; /* Darker shade for hover */
}
</style>
