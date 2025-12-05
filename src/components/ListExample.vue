<script setup>
import { ref, computed } from 'vue'

const todos = ref([
  { id: 1, text: 'Learn Vue 3 basics', completed: true },
  { id: 2, text: 'Understand Composition API', completed: true },
  { id: 3, text: 'Build a real project', completed: false },
  { id: 4, text: 'Master Vue Router', completed: false }
])

const newTodo = ref('')
const filter = ref('all') // 'all', 'active', 'completed'

const addTodo = () => {
  if (newTodo.value.trim()) {
    todos.value.push({
      id: Date.now(),
      text: newTodo.value,
      completed: false
    })
    newTodo.value = ''
  }
}

const toggleTodo = (id) => {
  const todo = todos.value.find(t => t.id === id)
  if (todo) {
    todo.completed = !todo.completed
  }
}

const removeTodo = (id) => {
  todos.value = todos.value.filter(t => t.id !== id)
}

const filteredTodos = computed(() => {
  if (filter.value === 'active') {
    return todos.value.filter(t => !t.completed)
  } else if (filter.value === 'completed') {
    return todos.value.filter(t => t.completed)
  }
  return todos.value
})
</script>

<template>
  <div class="list-example">
    <h2>List Rendering & Conditional Display (v-for, v-if)</h2>
    
    <div class="todo-app">
      <div class="add-todo">
        <input 
          v-model="newTodo" 
          @keyup.enter="addTodo"
          placeholder="Add a new todo..."
        />
        <button @click="addTodo">Add</button>
      </div>

      <div class="filters">
        <button 
          @click="filter = 'all'"
          :class="{ active: filter === 'all' }"
        >
          All
        </button>
        <button 
          @click="filter = 'active'"
          :class="{ active: filter === 'active' }"
        >
          Active
        </button>
        <button 
          @click="filter = 'completed'"
          :class="{ active: filter === 'completed' }"
        >
          Completed
        </button>
      </div>

      <div v-if="filteredTodos.length === 0" class="empty">
        <p>No todos to display!</p>
      </div>

      <ul v-else class="todo-list">
        <li 
          v-for="todo in filteredTodos" 
          :key="todo.id"
          :class="{ completed: todo.completed }"
        >
          <input 
            type="checkbox" 
            :checked="todo.completed"
            @change="toggleTodo(todo.id)"
          />
          <span>{{ todo.text }}</span>
          <button @click="removeTodo(todo.id)" class="remove">×</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.list-example {
  padding: 20px;
  background: #f5f5f5;
  border-radius: 8px;
  margin: 20px 0;
}

.todo-app {
  background: white;
  padding: 20px;
  border-radius: 6px;
}

.add-todo {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.add-todo input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.add-todo button {
  padding: 10px 20px;
  background: #42b883;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 600;
}

.add-todo button:hover {
  background: #369970;
}

.filters {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.filters button {
  padding: 8px 16px;
  background: #f0f0f0;
  border: 1px solid #ddd;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.2s;
}

.filters button.active {
  background: #42b883;
  color: white;
  border-color: #42b883;
}

.empty {
  text-align: center;
  color: #999;
  padding: 40px;
}

.todo-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.todo-list li {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 12px;
  border-bottom: 1px solid #eee;
  transition: background 0.2s;
}

.todo-list li:hover {
  background: #f9f9f9;
}

.todo-list li.completed span {
  text-decoration: line-through;
  color: #999;
}

.todo-list li input[type="checkbox"] {
  cursor: pointer;
}

.todo-list li span {
  flex: 1;
}

.remove {
  background: #ff4444;
  color: white;
  border: none;
  border-radius: 4px;
  width: 24px;
  height: 24px;
  cursor: pointer;
  font-size: 18px;
  line-height: 1;
}

.remove:hover {
  background: #cc0000;
}
</style>
