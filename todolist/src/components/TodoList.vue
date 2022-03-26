<template>
  <div class="card">
    <div
      class="card-body p-2 d-flex align-items-center"
      v-for="(todo, idx) in todos"
      :key="todo.id"
    >
      <div class="form-check flex-grow-1">
        <input
          class="form-check-input"
          type="checkbox"
          :checked="todo.completed"
          @change="toggleTodo(idx)"
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
</template>

<script>
export default {
  props: {
    todos: {
      type: Array,
      required: true,
    },
  },
  // emit을 기재해 주어야 함
  emits: ["toggle-todo", "delete-todo"],

  setup(props, { emit }) {
    const toggleTodo = (idx) => {
      emit("toggle-todo", idx);
    };
    const deleteTodo = (idx) => {
      emit("delete-todo", idx);
    };

    return {
      toggleTodo,
      deleteTodo,
    };
  },
};
</script>

<style></style>
