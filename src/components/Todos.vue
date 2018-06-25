<template>
  <section class="todoapp">
    <header class="header">
       <h1>TODOS</h1>
      <input type="text" class="new-todo" placeholder="Add Task" v-model="newTodo" @keyup.enter="addTodo"/>
    </header>
    <div class="main">
      <input type="checkbox" class="toggle-all" v-model="allDone"/>
      <ul class="todo-list">
       <li class="todo" v-for="todo in filteredTodos" :class="{completed: todo.completed, editing: todo == editing}">
         <div class="view">
           <input type="checkbox" v-model="todo.completed" class="toggle"/>
           <label @dblclick="editTodo(todo)">{{ todo.name }}</label>
           <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
         </div>
         <input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEdit" @keyup.esc="cancelEdit" v-focus="todo == editing" @blur="doneEdit"/>
       </li>
      </ul>
    </div>
    <footer class="footer" v-show="hasTodos">
      <span class="todo-count">
         <strong>{{ remaining}}</strong> Task left
      </span>
      <ul class="filters">
        <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter= 'all'">All Task</a></li>
        <li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter= 'todo'">Task left</a></li>
        <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter= 'done'">Task Completed</a></li>
      </ul>
      <button class="clear-completed" v-show="doneTodos" @click.prevent="deleteAllTodos">Delete all</button>
    </footer>
  </section>
</template>

<script>
  import Vue from 'vue'
  export default {
    data () {
      return {
        todos: [],
        newTodo: '',
        filter: 'all',
        editing: null,
        oldTodo: ''
      }

    },
    methods: {
      addTodo (){
        this.todos.push({
          completed: false,
          name: this.newTodo
        });
        this.newTodo = ''
      },
      deleteTodo (todo) {
        this.todos = this.todos.filter(i => i !== todo)
      },
      deleteAllTodos () {
        this.todos = this.todos.filter(todo => !todo.completed)
      },
      editTodo (todo) {
        this.editing = todo;
        this.oldTodo = todo.name
      },
      doneEdit (){
        this.editing = null

      },
      cancelEdit () {
        this.editing.name = this.oldTodo;
        this.doneEdit()
      }
    },
    directives: {
      focus (el, value) {
         if (value) {
          Vue.nextTick(_ => {
            el.focus()
          })
         }
      }

    },
    computed: {
      allDone: {
        get () {

        },
        set (value) {
          if (value === true) {
            this.todos.forEach( todo => {
              todo.completed = value
            })
          }
        }
      },
      hasTodos () {
        return this.todos.length > 0
      },
      doneTodos () {
        return this.todos.filter(todo => todo.completed).length
      },
      remaining () {
        return this.todos.filter(todo => !todo.completed).length
      },
      filteredTodos () {
        if (this.filter === 'todo')
        {
          return this.todos.filter(todo => !todo.completed)
        } else if (this.filter === 'done')
        {
          return this.todos.filter(todo => todo.completed)
        }

        return this.todos
      }
    }
  }
</script>

<style src="./todos.css"></style>
