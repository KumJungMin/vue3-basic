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
import axios from "axios";

export default {
  components: { TodoSimepleForm, TodoList },
  setup() {
    const todos = reactive([]);
    const deleteTodo = async (idx) => {
      const id = todos[idx].id;
      await axios.delete(`http://localhost:3000/todos/${id}`);
      todos.splice(idx, 1);
    };

    const getTodos = async () => {
      try {
        const { data } = await axios.get("http://localhost:3000/todos");
        Object.assign(todos, data); // reactive의 value를 변경하는 법
      } catch (e) {
        console.error(e);
      }
    };
    getTodos();

    const addTodo = async (todo) => {
      await axios.post("http://localhost:3000/todos", {
        subject: todo.subject,
        completed: todo.completed,
      });
      todos.push(todo);
    };
    const toggleTodo = async (idx) => {
      const id = todos[idx].id;
      await axios.patch(`http://localhost:3000/todos/${id}`, {
        completed: !todos[idx].completed,
      });
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
