<template>
  <h2>To-Do List</h2>
  <div class="container">
    <TodoSimepleForm @add-todo="addTodo" />
    <div v-if="!todos.length">추가된 Todo가 없습니다.</div>
    <div v-else class="card">
      <div
        class="card-body p-2 d-flex align-items-center"
        v-for="(todo, idx) in todos"
        :key="todo.id"
      >
        <div class="form-check flex-grow-1">
          <input
            class="form-check-input"
            type="checkbox"
            v-model="todo.completed"
          />
          <label class="form-check-label" :class="{ todo: todo.completed }">{{
            todo.subject
          }}</label>
        </div>
        <div>
          <button class="btn btn-danger btn-sm" @click="deleteTodo(idx)">
            Delete
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive } from "vue";
import TodoSimepleForm from "./components/TodoSimpleForm.vue";

export default {
  components: { TodoSimepleForm },
  setup() {
    const todos = reactive([]);
    const deleteTodo = (idx) => {
      todos.splice(idx, 1);
    };
    const addTodo = (todo) => {
      todos.push(todo);
    };
    return {
      todos,
      deleteTodo,
      addTodo,
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
