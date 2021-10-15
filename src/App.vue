<template>
  <div id="app">
    <h1>Todo List</h1>
    <form @submit.prevent="addTodo">
      <input v-model.trim="newTodo" placeholder="Enter new task" />
      <button :disabled="!newTodo">Add Todo</button>
    </form>
    <template v-if="todos.length > 0">
      <button @click="clearAllTodos" class="clear">Clear All Todos</button>
      <div v-for="todo in todos" :key="todo.id" class="todo" :class="{ 'completed': todo.completed }">
        <span v-if="!todo.editing" @click="toggleCompleted(todo)" class="title">
          {{ todo.title }}
        </span>
        <input v-else v-model="todo.title" @keydown.enter="toggleEditing(todo)" />
        <button @click="toggleEditing(todo)">{{ todo.editing ? 'Save' : 'Edit' }}</button>
        <button @click="deleteTodo(todo.id)">Delete</button>
      </div>
    </template>
    <h3 v-else>No Todos!</h3>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      newTodo: '',
      todos: JSON.parse(localStorage.getItem('todos')) || [],
    }
  },
  methods: {
    addTodo() {
      if (!this.newTodo) return;
      this.todos.push({
        id: Date.now(),
        title: this.newTodo,
        completed: false,
        editing: false,
      });
      this.newTodo = '';
    },
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    clearAllTodos() {
      this.todos = [];
    },
    toggleEditing(todo) {
      todo.editing = !todo.editing;
    },
    toggleCompleted(todo) {
      todo.completed = !todo.completed;
    },
  },
  watch: {
    todos: {
      handler() {
        localStorage.setItem('todos', JSON.stringify(this.todos));
      },
      deep: true,
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.clear, .title {
  margin: 5px;
}
.todo.completed .title {
  text-decoration: line-through;
}
</style>
