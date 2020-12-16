<template>
  <div id="app">
    <Header />
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="delTodo" />
    <img alt="Vue logo" src="./assets/logo.png" />
  </div>
</template>

<script>
import Todos from "./components/Todos";
import Header from "./components/layout/Header";
import AddTodo from "./components/AddTodo";
import axios from "axios";

export default {
  name: "App",
  components: {
    Todos,
    Header,
    AddTodo,
  },
  data() {
    return {
      todos: [],
    };
  },
  methods: {
    delTodo(id) {
      axios
        .delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
        .then(res => {this.todos = this.todos.filter(todo => todo.id!==id), console.log(res)} )
        .catch((err) => console.log(err));
      // this.todos = this.todos.filter((todo) => todo.id != id);
    },
    addTodo(newTodo) {
      const { title, completed } = newTodo;
      axios
        .post("https://jsonplaceholder.typicode.com/todos", {
          title,
          completed,
        })
        .then((res) => (this.todos = [...this.todos, res.data]))
        .catch((err) => console.log(err));
      this.todos = [...this.todos, newTodo];
    },
  },
  created() {
    console.log("created");
    axios
      .get("https://jsonplaceholder.typicode.com/todos?_limit=5")
      .then((res) => (this.todos = res.data))
      .catch((err) => console.log(err));
  },
};
</script>

<style>
</style>
