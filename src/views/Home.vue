<template>
  <div id="app">
    <AddTodo v-on:add-todo="addTodo" />
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo" />
  </div>
</template>

<script>
import Todos from '../components/Todos';
import AddTodo from '../components/AddTodo';
import axios from 'axios';

export default {
  name: 'Home',
  components: {
    Todos,
    AddTodo,
  },
  data() {
    return {
      todos: []
    }
  },
  methods: {
    deleteTodo(id) {
      axios.delete('https://jsonplaceholder.typicode.com/todos/'+id)
      .then(this.todos = this.todos.filter(todo => todo.id !== id))
      .catch(error => console.log(error))

      ;
    },
    addTodo(newTodo) {
      const { title, completed } = newTodo;
      // fetch add todo 
      axios.post('https://jsonplaceholder.typicode.com/todos', {
        title,
        completed
      })
      .then(response => this.todos = [...this.todos, response.data])
      .catch(error => console.log(error))
    }
  },
  created() {
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5')
    .then(response => this.todos = response.data)
    .catch(error => console.log(error))
  }
}
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;

  }

  .btn:hover {
    background: green;
  }
</style>
