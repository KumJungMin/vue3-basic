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

    const numberOfPages = computed(() => {
      return Math.ceil(numberOfTodos.value / limit);
    })


    const deleteTodo = async (idx) => {
      const id = todos[idx].id;
      await axios.delete(`http://localhost:3000/todos/${id}`);
      todos.splice(idx, 1);
    };

    const getTodos = async (page = currentPage.value) => {
      currentPage.value = page;
      try {
        const { data, headers } = await axios.get(`http://localhost:3000/todos?_page=${page}&_limit=${limit}`);
        Object.assign(todos, data); // reactive의 value를 변경하는 법
        numberOfTodos.value = headers['x-total-count'];
      } catch (e) {
        console.error(e);
      }
    };
    getTodos();

    // watch: 하나의 반응형 변수의 변화를 감지
    // ref 변수를 감지하는 방법
    watch(currentPage, (curr, prev) => {
      console.log('hello', curr, prev)
    })

    // reactive 변수를 감지하는 방법
    const test = reactive({age: 1});
    watch(() =>  test.age, (curr, prev) => {
      console.log(curr, prev)
    })
    test.age = 15;

    // 여러 개를 watch하는 방법 
    const test1 = ref(1);
    const test2 = ref(2);
    watch([test1, test2], ([new1, new2], [prev1, prev2]) => {
      console.log('test1', new1, prev1);
      console.log('test2', new2, prev2);
    })
    test1.value = 4;
    test2.value = 10;

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
      numberOfPages,
      currentPage,
      getTodos,
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
