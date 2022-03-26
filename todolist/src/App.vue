<template>
  <h2>To-Do List</h2>
  <div class="container">
    <input
      class="form-control"
      v-model="searchText"
      type="text"
      placeholder="Search"
    />
    <hr />
    <TodoSimepleForm @add-todo="addTodo" />
    <div v-if="!filteredTodos.length">추가된 Todo가 없습니다.</div>
    <TodoList
      :todos="filteredTodos"
      v-else
      @toggle-todo="toggleTodo"
      @delete-todo="deleteTodo"
    />
  </div>
</template>

<script>
import { reactive, computed, ref } from "vue";
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
    const searchText = ref("");
    const filteredTodos = computed(() => {
      if (searchText.value) {
        return todos.filter((todo) => todo.subject.includes(searchText.value));
      }
      return todos;
    });
    return {
      todos,
      deleteTodo,
      addTodo,
      toggleTodo,
      searchText,
      filteredTodos,
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
