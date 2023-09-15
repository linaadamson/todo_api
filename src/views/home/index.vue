<template>
  <main class="home">
    <div class="home-wrapper">
      <h1 class="header-text">Todo App</h1>
      <div class="input-wrapper">
        <!-- INPUT ELEMENT -->
        <TodoInput @add="addNewTodo($event)" />

        <!-- FILTER -->
        <TodoFilter :filterOptions="filterOptions" :filter="filter" @addFilters="selectedFilter($event)" />
      </div>

      <!-- CARDS -->
      <ul v-if="filteredTodos.length">
        <li v-for="(todo, index) in filteredTodos" v-bind:key="index">
          <TodoCard :todo="todo" @delete="deleteTodo($event)" @updateStatus="updateTodoStatus($event)" />
        </li>
      </ul>
      <h3 v-else class="no-todos">Nothing to see here, either create a new todo or go to bed</h3>
    </div>
  </main>
</template>

<script>
import TodoFilter from '../../components/todo-filter/index.vue'
import TodoCard from '../../components/todo-card/index.vue'
import TodoInput from '../../components/todo-input/index.vue'

import { v4 as uuidv4 } from 'uuid';
import axios from "axios";

export default {
  name: 'HomeView',
  components: {
    TodoInput,
    TodoCard,
    TodoFilter
  },
  data() {
    return {
      filter: 'all',
      todos: [],
      filterOptions: [
        { value: 'all', label: 'all' },
        { value: true, label: 'completed' },
        { value: false, label: 'uncompleted' },

      ],
    }
  },

  methods: {
    getTodoList: async function () {
      const url = 'https://jsonplaceholder.typicode.com/todos/'
      try {
        const { data } = await axios.get(url);
        this.todos = data
      }
      catch (error) {
        console.error(error);
      }
    },

    addNewTodo: async function (todo) {
      // this.todos.push({ id: uuidv4(), title: todo, completed: false })
      try {
        const res = await axios.post("https://jsonplaceholder.typicode.com/todos", {
          userId: uuidv4(),
          title: todo,
          completed: false,
        });
        console.log(res)
      } catch (error) {
        console.log(error);
      }
    },

    deleteTodo: async function (todo) {
      // this.todos = this.todos.filter((item) => { return item.id !== todo.id })
      try {
        const res = await axios.delete("https://jsonplaceholder.typicode.com/todos/" + todo.id)
        console.log(res)
      } catch (error) {
        console.log(error);
      }
    },

    updateTodoStatus: async function (todo) {
      // const todoIndex = this.todos.findIndex((item) => item.id === todo.id)
      // this.todos[todoIndex].completed = todo.completed === true ? false : true;

      try {
        const res = await axios.patch("https://jsonplaceholder.typicode.com/todos/" + todo.id, {
          completed: todo.completed === true ? false : true,
        })
        console.log(res)
      } catch (error) {
        console.log(error);
      }
    },
    selectedFilter: function (e) {
      this.filter = e
    }
  },
  computed: {
    filteredTodos: function () {
      return this.todos.filter((item) => {

        if (this.filter === 'all') {
          return item
        }
        else {
          return item.completed === (this.filter == 'true' ? true : false)
        }

      }).reverse();
    }
  },

  created() {
    this.getTodoList()

  }

}
</script>


<style lang="css" scoped>
@import './index.css';
</style>

