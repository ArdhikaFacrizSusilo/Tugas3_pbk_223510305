<template>
    <div class="todo-list">
      <h1>DAFTAR KEGIATAN</h1>
      <input type="text" v-model="newTodoTitle" @keyup.enter="addTodo" placeholder="Tambahkan Kegiatan">
  
      <h2>Kegiatan Yang Tertunda</h2>
      <ul>
        <li v-for="todo in pendingTodos" :key="todo.id">
          <TodoItem :todo="todo" @update-todo="updateTodo" @delete-todo="deleteTodo" />
        </li>
      </ul>
  
      <div class="show-completed">
        <input type="checkbox" v-model="showCompleted" id="showCompleted">
        <label for="showCompleted">Tampilkan Tugas Yang Selesai</label>
      </div>
  
      <div v-if="showCompleted">
        <ul>
          <li v-for="todo in completedTodos" :key="todo.id">
            <TodoItem :todo="todo" @update-todo="updateTodo" @delete-todo="deleteTodo" />
          </li>
        </ul>
      </div>
    </div>
  </template>
  
  <script>
  import TodoItem from './TodoItem.vue';
  
  export default {
    components: {
      TodoItem
    },
    data() {
      return {
        newTodoTitle: '',
        todos: [],
        showCompleted: false
      };
    },
    computed: {
      pendingTodos() {
        return this.todos.filter(todo => !todo.completed);
      },
      completedTodos() {
        return this.todos.filter(todo => todo.completed);
      }
    },
    methods: {
      addTodo() {
        if (this.newTodoTitle.trim() === '') return;
        this.todos.push({
          id: Date.now(),
          title: this.newTodoTitle,
          completed: false
        });
        this.newTodoTitle = '';
      },
      updateTodo(updatedTodo) {
        const index = this.todos.findIndex(todo => todo.id === updatedTodo.id);
        if (index !== -1) {
          this.todos.splice(index, 1, updatedTodo);
        }
      },
      deleteTodo(todoToDelete) {
        this.todos = this.todos.filter(todo => todo.id !== todoToDelete.id);
      }
    }
  };
  </script>
  
  <style scoped>
  .todo-list {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
    border: 1px solid #e0e0e0;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    background-color: #ffffff;
  }
  .todo-list h1 {
    text-align: center;
    color: #34495e;
    margin-bottom: 20px;
  }
  .todo-list input[type="text"] {
    width: 100%;
    padding: 12px;
    border: 1px solid #ddd;
    border-radius: 5px;
    margin-bottom: 20px;
    box-sizing: border-box;
    font-size: 16px;
  }
  .todo-list ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  .todo-list li {
    margin-bottom: 10px;
  }
  .show-completed {
    display: flex;
    align-items: center;
    margin-top: 20px;
  }
  .show-completed input[type="checkbox"] {
    margin-right: 10px;
  }
  .show-completed label {
    font-size: 16px;
    color: #2c3e50;
  }
  </style>
  