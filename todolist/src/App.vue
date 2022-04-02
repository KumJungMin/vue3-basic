<template>
  <h2>To-Do List</h2>
  <div class="container">
    <input
      class="form-control"
      v-model="searchText"
      type="text"
      placeholder="Search"
      @keyup.enter="searchTodo"
    />
    <hr />
    <TodoSimepleForm @add-todo="addTodo" />
    <div v-if="!todos.length">추가된 Todo가 없습니다.</div>
    <TodoList
      :todos="todos"
      v-else
      @toggle-todo="toggleTodo"
      @delete-todo="deleteTodo"
    />
    <hr />
    <nav aria-label="Page navigation example">
      <ul class="pagination">
        <li v-if="currentPage !== 1" class="page-item">
          <a class="page-link" @click.stop="getTodos(currentPage - 1)">Previous</a>
        </li>
        <li 
          v-for="page in numberOfPages" 
          class="page-item" 
          :key="page" 
          :class="{'active': currentPage === page}"
        >
          <a class="page-link" @click.stop="getTodos(page)">{{page}}</a>
        </li>
        <li v-if="currentPage !== numberOfPages" class="page-item">
          <a class="page-link" @click.stop="getTodos(currentPage + 1)">Next</a>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
import { reactive, computed, ref, watch } from "vue";
import TodoSimepleForm from "./components/TodoSimpleForm.vue";
import TodoList from "./components/TodoList.vue";
import axios from "axios";

export default {
  components: { TodoSimepleForm, TodoList },
  setup() {
    const todos = reactive([]);
    const numberOfTodos = ref(0);
    const limit = 5;
    const currentPage = ref(1);
    const searchText = ref("");

    const numberOfPages = computed(() => {
      return Math.ceil(numberOfTodos.value / limit);
    })


    const deleteTodo = async (idx) => {
      const id = todos[idx].id;
      await axios.delete(`http://localhost:3000/todos/${id}`);
      getTodos(1);
    };

    const getTodos = async (page = currentPage.value) => {
      currentPage.value = page;
      try {
        const { data, headers } = await axios.get(`http://localhost:3000/todos?_sort=id&_order=desc&subject_like=${searchText.value}&_page=${page}&_limit=${limit}`);
        Object.assign(todos, data); // reactive의 value를 변경하는 법
        numberOfTodos.value = headers['x-total-count'];
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
      getTodos(1)
    };
    const toggleTodo = async (idx) => {
      const id = todos[idx].id;
      await axios.patch(`http://localhost:3000/todos/${id}`, {
        completed: !todos[idx].completed,
      });
      todos[idx].completed = !todos[idx].completed;
    };

    let timeout = null;
    const searchTodo =()=> {
      clearTimeout(timeout);
      getTodos(1);
    }

    watch(searchText, () => {
      clearTimeout(timeout);
      timeout = setTimeout(() => {
        getTodos(1);
      }, 1000)
    })

    return {
      todos,
      deleteTodo,
      addTodo,
      toggleTodo,
      searchText,
      numberOfPages,
      currentPage,
      getTodos,
      searchTodo,
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
