<script setup>
import Navbar from './components/Navbar.vue';
import List from './components/List.vue';
</script>

<template>
  <!-- Navbar -->
  <navbar />
  <div class="container">
    <div class="row mt-4">
      <div class="col-md-8 offset-2">
        <div class="input-group mb-3">
          <input type="text" class="form-control" v-model="todo" placeholder="Add to do" @keyup.enter="add">
          <button class="btn btn-outline-primary" type="button" @click="add">Add</button>
        </div>
      </div>
    </div>
    <div class="row mt-3">
      <div class="col-md-8 offset-2">
        <list 
          :todos="todos"
          @emitDone="done"
          @emitDestroy="destroy"
        />
        <small>Total To Do : {{ totalTodo }}</small>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  components: {Navbar, List},
  data() {
    return {
      todo: "",
      todos: []
    }
  },
  mounted() {
    this.todos = JSON.parse(localStorage.getItem('todos')) ?? []
  },
  computed: {
    totalTodo() {
      return this.todos.length
    }
  },
  methods: {
    add() {
      this.todos.unshift({activity: this.todo, isDone:false})
      this.todo = ""
      this.saveToLocalStorage()
    },
    done(todoIndex) {
      this.todos = this.todos.filter((item, index) => {
        if (index == todoIndex) {
          item.isDone = !item.isDone
        }

        return item
      })
      this.saveToLocalStorage()
    },
    destroy(todoIndex) {
      this.todos = this.todos.filter((item, index) => {
        if (index != todoIndex) {
          return item
        }
      })
      this.saveToLocalStorage()
    },
    saveToLocalStorage() {
      localStorage.setItem('todos', JSON.stringify(this.todos))
    }
  }
}
</script>