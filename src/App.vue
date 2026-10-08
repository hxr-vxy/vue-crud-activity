<script setup>
import { ref, onMounted } from 'vue'
import Queue from './components/Queue.vue'

const queue = ref([])

const newTodo = ref('')

function addQueue() {
  if (newTodo.value.trim() === '') {
    return
  }

  queue.value.push({
    id: Date.now(),
    title: newTodo.value,
    completed: false
  })

  newTodo.value = ''

  saveTodos()
}

function deleteQueue(id) {
  queue.value = queue.value.filter(todo => todo.id !== id)

  saveTodos()
}

function updateQueue(updatedQueue) {
  const index = queue.value.findIndex(todo => todo.id === updatedQueue.id)

  if (index !== -1) {
    queue.value[index] = updatedQueue
  }

  saveTodos()
}

function saveTodos() {
  localStorage.setItem('todos', JSON.stringify(queue.value))
}

onMounted(() => {
  const savedTodos = localStorage.getItem('todos')

  if (savedTodos) {
    queue.value = JSON.parse(savedTodos)
  }
})
</script>

<template>
  <div class="container">

    <h1>Queueing</h1>

    <!-- Add Queue -->
    <div class="add-queue">
      <input
        v-model="newTodo"
        type="text"
        placeholder="Enter your name"
        @keyup.enter="addQueue"
      />

      <button @click="addQueue">
        Add
      </button>
    </div>

    <!-- Todo List -->
    <div class="todo-list">

      <Queue
        v-for="queueItem in queue"
        :key="queueItem.id"
        :queue="queueItem"
        @delete="deleteQueue"
        @update="updateQueue"
      />

    </div>

  </div>
</template>