<script setup>
import { ref, onMounted } from 'vue'
import Queue from './components/Queue.vue'

const queue = ref([])

const newQueue = ref('')

function addQueue() {
  if (newQueue.value.trim() === '') {
    return
  }

  queue.value.push({
    id: Date.now(),
    title: newQueue.value,
    completed: false
  })

  newQueue.value = ''

  save()
}

function deleteQueue(id) {
  queue.value = queue.value.filter(queue => queue.id !== id)

  save()
}

function updateQueue(updatedQueue) {
  const index = queue.value.findIndex(queue => queue.id === updatedQueue.id)

  if (index !== -1) {
    queue.value[index] = updatedQueue
  }

  save()
}

function save() {
  localStorage.setItem('queues', JSON.stringify(queue.value))
}

onMounted(() => {
  const savedQueues = localStorage.getItem('queues')

  if (savedQueues) {
    queue.value = JSON.parse(savedQueues)
  }
})
</script>

<template>
  <div class="container">

    <h1>Queueing</h1>

    <!-- Add Queue -->
    <div class="add-queue">
      <input
        v-model="newQueue"
        type="text"
        placeholder="Enter your name"
        @keyup.enter="addQueue"
      />

      <button @click="addQueue">
        Add
      </button>
    </div>

    <!-- Queue List -->
    <div class="queue-list">

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