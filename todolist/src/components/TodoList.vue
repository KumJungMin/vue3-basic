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
</template>

<script>
export default {
  props: {
    todos: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style></style>
