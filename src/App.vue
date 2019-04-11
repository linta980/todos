<template>

  <div id="app">
   <Header />
   <AddTodo v-on:add-todo="addToDO"/>
    <Todos v-bind:todos="todos" v-on:del-todo="deleteTodo"/>
  </div>
</template>



<script>
//Ovde imam komponentu HelloWorld koju gore koristim kao i sve ostale komponente koje budem dodavaoi moram da je importujem pre ovoga
//data() je funkcija ko vraca objekat
// {{msg}} se vuce odozdo iz data()
//todos je array of objects

import Todos from './components/Todos.vue';
import Header from './components/layout/Header.vue'
import AddTodo from './components/AddTodo.vue'
import axios from 'axios';


export default {
  name: "app",
  components: {
    Todos,
    Header,
    AddTodo

  },
  methods:{
    deleteTodo(id){
      // this.todos = this.todos.filter(todo => todo.id !==id) --sad koristim axios
      axios.delete(`https://jsonplaceholder.typicode.com/todos/${id}`)
      .then(res => this.todos = this.todos.filter(todo => todo.id !==id))
      .catch(error => console.log(error))
    },
    addToDO(newToDo){
     // this.todos = [...this.todos, newToDo]; --sad koristim axios
    const {title, completed} =newToDo;
    axios.post('https://jsonplaceholder.typicode.com/todos',{
      title,
      completed
    })
    .then(res =>this.todos = [...this.todos ,res.data])
    .catch(error => console.log(error))
    }
  },
  created(){
    axios.get('https://jsonplaceholder.typicode.com/todos?_limit=5')
    .then(res=> this.todos=res.data )
    .catch(error => console.log(error))
  },
  data() {
    return {
      todos: [
        // {
        //   id: 1,
        //   title: "Todo One",
        //   completed: false
        // },
        // {
        //   id: 2,
        //   title: "Todo Two",
        //   completed: true
        // },
        // {
        //   id: 3,
        //   title: "Todo Three",
        //   completed: false
        // }
      ]
    };
  }
};






</script>
// Ovo ovde je Globalni stil za sve , nema scoped u style-u
<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
  body {
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
  }
  .btn {
    display: inline-block;
    border: none;
    background: #555;
    color: #fff;
    padding: 7px 20px;
    cursor: pointer;
  }
  .btn:hover {
    background: #666;
  }
</style>
