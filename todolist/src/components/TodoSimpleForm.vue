<template>
  <form class="d-flex" @submit.prevent="onSubmit">
    <div class="flex-grow-1 mr-2">
      <input
        class="form-control"
        v-model="todo"
        type="text"
        placeholder="Type new to-do"
      />
    </div>
    <div>
      <button class="btn btn-primary">Add</button>
    </div>
  </form>
  <div v-show="hasError" class="error">This field cannot be empty</div>
</template>

<script>
import { ref } from "vue";
export default {
  emits: ["add-todo"],
  setup(props, { emit }) {
    const todo = ref("");
    const hasError = ref(false);
    const onSubmit = () => {
      if (todo.value === "") hasError.value = true;
      else {
        const newTodo = {
          id: Date.now(),
          subject: todo.value,
          completed: false,
        };
        emit("add-todo", newTodo);
        hasError.value = false;
        todo.value = "";
      }
    };

    return { todo, hasError, onSubmit };
  },
};
</script>

<style></style>
