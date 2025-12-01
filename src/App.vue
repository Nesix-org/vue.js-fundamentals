<script setup>
import { ref, computed, onMounted, onUpdated } from 'vue'

const count = ref(0)
const text = ref('')

let id = 0
const newTodo = ref('')
const hideCompletedTodos = ref(false)
const todos = ref([
  { id: id++, task: 'sing', done: true },
  { id: id++, task: 'dance', done: false }
])

function addTodo() {
  todos.value.push({ id: id++, task: newTodo.value, done: false })
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}

const awesome = ref(true)

function toggle() {
  awesome.value = !awesome.value
}

// v-bind, v-on, v-if/v-else, v-model

// why put this in a computed? so the list auto-updates anytime you add a todo, mark
// a todo as done or you toggle hideCompletedTodos
const filteredTodos = computed(() => {
  return hideCompletedTodos.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})

onMounted(() => console.log('mounted'))
onUpdated(() => console.log('updated'))
</script>


<template>
  <section>
    <img   />
    <h1> {{ count }} </h1>
    <input />
    <div> {{ text }} </div>
    <button @click="toggle">Toggle</button>
    <h1 v-if="awesome">Vue is awesome!</h1>
    <h1 v-else>Oh no 😢</h1>
    <h1 class="text-2xl">Todo App</h1>
    <form @submit.prevent="addTodo">
      <input class="" v-model="newTodo" required placeholder="Add Todo" />
      <button class="block"> Add Todo</button>
      <ol>
        <li v-for="todo in filteredTodos" :key="todo.id">
          <input type="checkbox" v-model="todo.done" />
          <span :class="{ done: todo.done }">
            {{ todo.task }}
          </span>
          <button @click="removeTodo(todo)"> X </button>
        </li>
      </ol>
    </form>
    <button @click="hideCompletedTodos = !hideCompletedTodos"> {{ hideCompletedTodos ? ' Uncompleted' : ' All Todos' }}
    </button>
  </section>
</template>

<style>
section {
  margin-inline: 500px;
  margin-block: 100px;
}
.done {
  text-decoration: line-through;
  color: green;
}

input, button {
  padding: 8px 10px;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 14px;
}

input:focus {
  border-color: #42b883; /* Vue green */
  outline: none;
}

button {
  background: #42b883;
  color: white;
  cursor: pointer;
  transition: 0.2s;
  margin-top: 8px;
}

button:hover {
  background: #36976f;
}

form {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  gap: 10px;
  max-width: 300px;
}

ol {
  padding-left: 20px;
}

li {
  margin-bottom: 8px;
  display: flex;
  align-items: center;
  gap: 10px;
}

.done {
  text-decoration: line-through;
  color: gray;
}

h1 {
  margin-top: 20px;
}

</style>
