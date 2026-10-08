<script setup>
import { ref } from 'vue'

const props = defineProps({
    queue: Object 
})

const emit = defineEmits([
 'delete',
 'update'   
])

const isEditing = ref(false)

const editedTitle = ref(props.queue.title)

function updateInfo() {
    if (editedTitle.value.trim() === '') {
        return
    }
    emit('update', {
        ...props.queue,
        title: editedTitle.value
    })

    isEditing.value = false
}

function toggleCompleted() {
    emit('update', {
        ...props.queue,
        completed: !props.queue.completed
    })

    
}

function deleteTask() {
    emit('delete', props.queue.id)
}

function startEditing() {
    editedTitle.value = props.queue.title
    isEditing.value = true
}


</script>


<template>

  <div class="queueing">

    <!-- Normal Mode -->
    <div v-if="!isEditing">

      <input
        type="checkbox"
        :checked="queue.completed"
        @change="toggleCompleted"
      />

      <span :class="{ completed: queue.completed }">
        {{ queue.title }}
      </span>

      <button @click="startEditing">
        Edit
      </button>

      <button @click="deleteTask">
        Delete
      </button>

    </div>

    <!-- Edit Mode -->
    <div v-else>

      <input
        v-model="editedTitle"
        @keyup.enter="updateInfo"
        
      />

      <button @click="updateInfo">
        Save
      </button>

      <button @click="isEditing = false">
        Cancel
      </button>

    </div>

  </div>

</template>


<style scoped>


.queueing {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}


.queueing span {
  margin: 0 10px;
}


.completed {
  text-decoration: line-through;
  color: gray;
}


button {
  margin-left: 5px;
}


</style>

