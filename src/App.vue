<script>
import { remove } from '@vue/shared'

let id = 0 

export default {
  data() {
    return {
      newTodo: '',
      hideCompleted: false,
      hideToDo: false,
      todos: [
        { id: id++, text: 'Crea un pendiente', done: true},
        { id: id++, text: 'Marca un pendiente como completado', done: false},
        { id: id++, text: 'Selecciona Mostrar completados', done: false}
      ]
    }
  },
  computed: {
    filteredCompleted() {
      return this.hideToDo
      ? this.todos.filter((t) => t.done)
      : this.todos
    },
    filteredToDos() {
      return this.hideCompleted
      ? this.todos.filter((t) => !t.done)
      : this.todos
    }
  },
  methods: {
    addTodo() {
      this.todos.push({id: id++, text: this.newTodo, done: false})
      this.newTodo = ''
    },
    removeTodo(todo){
      this.todos = this.todos.filter((t) => t !== todo)
    }
  }
}
</script>

<template>
  <section class="cabecera">
      <h1 class="titulo">Lista de Pendientes</h1>
  </section>
  <section class="crear-pendiente">
      <h3>¿ALGUN PENDIENTE?</h3>
      <form id="nuevo-pendiente" @submit.prevent="addTodo">
          <input type="text" id="content" placeholder="Escribe tu pendiente aqui" v-model="newTodo">
          <button class="agregar">Agregar Pendiente</button>
      </form>
  </section>
  <section class="lista-pend">
      <h3>LISTA DE PENDIENTES</h3>
      <div class="shown-button">
          <button class="actions" @click="hideToDo = !hideToDo">
              {{ hideToDo ? 'Mostrar Todo' : 'Mostrar Solo Completados' }}
          </button>
          <button class="actions" @click="hideCompleted = !hideCompleted">
              {{ hideCompleted ? 'Mostrar Todo' : 'Mostrar Solo Pendientes' }}
          </button>
      </div>
      <div class="lista">
            <ul v-if="hideCompleted">
                <li v-for="todo in filteredToDos" :key="todo.id" :class="`todo-item ${todo.done && 'done'}`">
                    <label>
                        <input type="checkbox" v-model="todo.done">
                        <span class="bubble"></span>
                    </label>
                    <div class="todo-content">
                        <span :class="{ done: todo.done }">{{ todo.text }}</span>
                    </div>
                    <div class="actions">
                        <button class="delete" @click="removeTodo(todo)">Eliminar</button>
                    </div>
                </li>
            </ul>
          <ul v-else-if="hideToDo">
              <li v-for="todo in filteredCompleted" :key="todo.id" :class="`todo-item ${todo.done && 'done'}`">
                <label>
                      <input type="checkbox" v-model="todo.done">
                      <span class="bubble"></span>
                  </label>
                  <div class="todo-content">
                      <span :class="{ done: todo.done }">{{ todo.text }}</span>
                  </div>
                  <div class="actions">
                      <button class="delete" @click="removeTodo(todo)">Eliminar</button>
                  </div>
              </li>
          </ul>
          <ul v-else>
              <li v-for="todo in filteredToDos" :key="todo.id" :class="`todo-item ${todo.done && 'done'}`">
                <label>
                      <input type="checkbox" v-model="todo.done">
                      <span class="bubble"></span>
                  </label>
                  <div class="todo-content">
                      <span :class="{ done: todo.done }">{{ todo.text }}</span>
                  </div>
                  <div class="actions">
                      <button class="delete" @click="removeTodo(todo)">Eliminar</button>
                  </div>
              </li>
          </ul>
      </div>
  </section>
</template>

<style>
.done {
  text-decoration: line-through;
}
</style>