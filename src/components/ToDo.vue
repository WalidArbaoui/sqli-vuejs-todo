<script setup lang="ts">
import { computed, ref, watch } from 'vue'
import { Icon } from '@iconify/vue'

let id = 0
const newTodo = ref()
const show = ref('all')
const todos = ref([
  { id: id++, text: 'Learn HTML', done: true },
  { id: id++, text: 'Learn JavaScript', done: true },
  { id: id++, text: 'Learn Vue', done: false },
])

const filteredTodos = computed(() => {
  if (show.value === 'pending') {
    return todos.value.filter((t) => !t.done)
  } else if (show.value === 'completed') {
    return todos.value.filter((t) => t.done)
  }
  return todos.value
})

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done: false })
  newTodo.value = ''
}

function removeTodo(todo: any) {
  todos.value = todos.value.filter((t) => t !== todo)
}
</script>

<template>
  <div class="container">
    <h1>ToDo Application</h1>
    <form @submit.prevent="addTodo" class="todo-form">
      <input v-model="newTodo" required placeholder="new todo" />
      <button>Add</button>
    </form>
    <ul v-if="filteredTodos.length">
      <li v-for="todo in filteredTodos" :key="todo.id">
        <div>
          <input :id="`todo${todo.id}`" type="checkbox" v-model="todo.done" />
          <label :for="`todo${todo.id}`" :class="{ done: todo.done }">{{ todo.text }}</label>
        </div>
        <button @click="removeTodo(todo)">
          <Icon icon="streamline:recycle-bin-2" color="white" />
        </button>
      </li>
    </ul>
    <div class="empty" v-else>
      <Icon icon="ph:empty" color="white" width="124" />
      <p>Nothing to show!</p>
    </div>
    <div class="filter">
      <h3>Show</h3>
      <button @click="show = 'all'" :class="{ active: show === 'all' }">All</button>
      <button @click="show = 'pending'" :class="{ active: show === 'pending' }">Pending</button>
      <button @click="show = 'completed'" :class="{ active: show === 'completed' }">
        Completed
      </button>
    </div>
  </div>
</template>

<style scoped>
.container {
  background-color: var(--vt-c-black-soft);
  height: 100%;
  border-radius: 24px;
  display: flex;
  flex-direction: column;
}
.container h1 {
  color: white;
  padding: 16px;
  border-bottom: 1px solid white;
}

.todo-form {
  display: flex;
  padding: 16px;
}
.todo-form input {
  color: white;
  padding: 8px 12px;
  font-size: 18px;
  background-color: var(--vt-c-black-mute);
  border: 1px solid gray;
  flex-grow: 1;
  border-radius: 999px 0 0 999px;
}
.todo-form input:focus {
  outline: 1px solid lightgray;
}
.todo-form button {
  background-color: var(--vt-c-black-mute);
  color: white;
  border: none;
  border: 1px solid gray;
  border-radius: 0 999px 999px 0;
  padding: 12px;
  font-size: 18px;
  cursor: pointer;
}

ul {
  flex: 1;
  list-style: none;
  padding: 0 24px;
  max-height: 600px;
  overflow-y: scroll;
}

.empty {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

ul li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  padding: 6px 0;
}
ul li:not(:last-of-type) {
  border-bottom: 1px solid gray;
}

ul li div {
  display: flex;
  align-items: center;
  gap: 6px;
}

ul li input {
  width: 18px;
  height: 18px;
}
ul li button {
  padding: 6px;
  background: none;
  border: none;
  cursor: pointer;
}

.filter {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  justify-content: center;
  gap: 16px;
  padding: 16px 32px;
  border-top: 1px solid white;
}

.filter button {
  background-color: var(--vt-c-black-mute);
  border: none;
  color: white;
  border-radius: 1000px;
}
.filter button {
  background-color: var(--vt-c-black-mute);
  border: none;
  color: white;
  border-radius: 1000px;
  cursor: pointer;
}

.filter button.active {
  outline: 1px solid white;
  outline-offset: 4px;
}
</style>
