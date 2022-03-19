<template>
  <h2>To-Do List</h2>
  <div class="container">
    <TodoSimepleForm @add-todo="addTodo" />
    <div v-if="!todos.length">추가된 Todo가 없습니다.</div>
    <TodoList
      :todos="todos"
      v-else
      @toggle-todo="toggleTodo"
      @delete-todo="deleteTodo"
    />
  </div>
</template>

<script>
import { reactive } from "vue";
import TodoSimepleForm from "./components/TodoSimpleForm.vue";
import TodoList from "./components/TodoList.vue";

export default {
  components: { TodoSimepleForm, TodoList },
  setup() {
    const todos = reactive([]);
    const deleteTodo = (idx) => {
      todos.splice(idx, 1);
    };
    const addTodo = (todo) => {
      todos.push(todo);
    };
    const toggleTodo = (idx) => {
      todos[idx].completed = !todos[idx].completed;
    };

    return {
      todos,
      deleteTodo,
      addTodo,
      toggleTodo,
    };
  },
};
</script>

<style>
.error {
  color: red;
}
.todo {
  color: gray;
  text-decoration: line-through;
}
</style>
