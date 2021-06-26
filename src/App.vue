<template>
  <div id="app">
    <h1>Todo List</h1>
    <h4>Adicionar Todo</h4>
    <form v-on:submit.prevent>
      <input type="text" placeholder="Escrever aqui um novo todo..." v-model="newTodo" />
      <button @click="addTodo" v-if="id === undefined">Adicionar Todo</button>
      <span>
        <button @click="cancel" v-if="id !== undefined">Cancelar</button>
        <button @click="editTodo" v-if="id !== undefined">Editar Todo</button>
      </span>
    </form>
    <div v-for="(todo, index) in todos" :key="todo.id" :id="'todo_' + todo.id" class="todo-item">
      <p @dblclick="edit(todo.id)">{{ todo.title }}</p>
      <button><img :src="trashCan" @click="removeTodo(index)"/></button>
    </div>
    
    <HelloWorld @deleteAll="deleteAll" />
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
// Import Images
import trashCan from '../src/assets/trash.svg'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  
  data () {
    return {
      newTodo: '',
      id: undefined,
      completed: undefined,
      todos:[
        {
          'id': 2,
          'title': 'Ir às compras',
          'completed': false,
        },
        {
          'id': 1,
          'title': 'Pagar a conta da luz',
          'completed': false,
        },
        {
          'id': 3,
          'title': 'Almoço com os amigos',
          'completed': false,
        }
      ],
      trashCan: trashCan
    }
  },
  methods: {
    saveList() {
      localStorage.setItem('savedList', JSON.stringify(this.todos))
    },
    addTodo() {
      if (this.newTodo.trim().length === 0) return
      this.todos.unshift({
        id: this.todos.length+1,
        title: this.newTodo.trim(),
        completed: false
      })

      this.newTodo = ""
      this.saveList()
    },
    removeTodo(index) {
      this.todos.splice(index, 1)
      this.cancel()
      this.saveList()
    },
    edit(id) {
      this.newTodo = this.todos.filter( e => e.id == id)[0].title
      this.id = this.todos.filter( e => e.id == id)[0].id
      this.completed = this.todos.filter( e => e.id == id)[0].completed
    },
    cancel() {
      this.newTodo = ''
      this.id = undefined
      this.completed = undefined
    },
    editTodo() {
      let editing = {
        title: this.newTodo,
        id: this.id,
        completed: this.completed
      }
      this.todos.splice(this.todos.map( e => { return e.id }).indexOf(this.id), 1, editing)
      this.cancel()
      this.saveList()
    },
    deleteAll() {
      this.todos = []
      this.saveList()
    }
  },
  created() {
    this.todos = JSON.parse(localStorage.getItem('savedList')) || []
    console.log(localStorage.getItem('savedList'))
  }
}
</script>

<style lang="scss" scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  box-sizing: border-box;
}
form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 400px;
  margin: auto;
  input {
    font-size: x-large;
    padding: 16px 16px;
    width: -webkit-fill-available;
  }
  button {
    margin-top: 4px;
    padding: 8px 16px;
    background-color: #188753;
    border: none;
    border-radius: 4px;
    color: #fff;
    margin-bottom: 16px;
    cursor: pointer;
  }
  > span > :first-child {
    background-color: grey;
  }
}
.todo-item {
  border-radius: 4px;
  margin: auto;
  display: flex;
  justify-content: center;
  padding: 4px 0px;
  border: 1px solid #d3d3d3;
  width: 400px;
  background-color: #fbfbfb;
  p {
    background-color: #fff;
    border: inherit;
    border-radius: inherit;
    width: 300px;
    margin: 0;
    padding: 8px;
    text-align: initial;
    font-size: x-large;
  }
  button {
    border: none;
    border-radius: inherit;
    background-color: #dc3545;
    padding: 4px;
    cursor: pointer;
    img {
      width: 25px;
      height: 25px;
      filter: invert(1);
      padding: 4px;
      
    }
  }
}

</style>
