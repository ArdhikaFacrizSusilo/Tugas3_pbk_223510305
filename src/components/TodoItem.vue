<template>
  <div class="todo-item">
    <input type="checkbox" v-model="todo.completed" @change="toggleComplete">
    <input
      v-if="isEditing"
      type="text"
      v-model="editText"
      @keyup.enter="stopEdit"
      @blur="stopEdit"
      ref="editInput"
    >
    <span
      v-else
      :class="{ completed: todo.completed }"
    >{{ todo.title }}</span>
    <button class="edit-btn" @click="startEdit">Edit</button>
    <button class="delete-btn" @click="deleteTodo">Delete</button>
  </div>
</template>

<script>
export default {
  props: ['todo'],
  data() {
    return {
      isEditing: false,
      editText: this.todo.title
    };
  },
  methods: {
    toggleComplete() {
      this.$emit('update-todo', this.todo);
    },
    startEdit() {
      this.isEditing = true;
      this.$nextTick(() => {
        this.$refs.editInput.focus();
      });
    },
    stopEdit() {
      if (this.editText.trim() === '') {
        this.editText = this.todo.title;
      } else {
        this.$emit('update-todo', { ...this.todo, title: this.editText });
      }
      this.isEditing = false;
    },
    deleteTodo() {
      this.$emit('delete-todo', this.todo);
    }
  },
  watch: {
    todo(newTodo) {
      this.editText = newTodo.title;
    }
  }
};
</script>

<style scoped>
.todo-item {
  display: flex;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #eee;
  background-color: #fafafa;
  border-radius: 5px;
  transition: background-color 0.3s;
}
.todo-item input[type="checkbox"] {
  margin-right: 10px;
}
.todo-item input[type="text"] {
  flex-grow: 1;
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 16px;
}
.todo-item span {
  flex-grow: 1;
  padding: 5px;
  font-size: 16px;
}
.todo-item span.completed {
  text-decoration: line-through;
  color: #888;
}
.todo-item .edit-btn, .todo-item .delete-btn {
  background-color: #3498db;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
  border-radius: 5px;
  margin-left: 5px;
  transition: background-color 0.3s;
}
.todo-item .edit-btn:hover {
  background-color: #2980b9;
}
.todo-item .delete-btn {
  background-color: #e74c3c;
}
.todo-item .delete-btn:hover {
  background-color: #c0392b;
}
.todo-item:hover {
  background-color: #f0f0f0;
}
</style>
