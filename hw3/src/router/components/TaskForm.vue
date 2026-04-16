<template>
  <form @submit.prevent="submitTask" class="task-form card">
    <h2>Add New Task</h2>
    <div class="form-group">
      <input v-model="title" type="text" placeholder="Task title..." required />
      
      <select v-model="priority">
        <option value="High">High Priority</option>
        <option value="Medium">Medium Priority</option>
        <option value="Low">Low Priority</option>
      </select>

      <input v-model="category" type="text" placeholder="Category (e.g., Work)" required />
      
      <button type="submit" class="btn btn-primary">Add Task</button>
    </div>
  </form>
</template>

<script setup>
import { ref } from 'vue'

const emit = defineEmits(['add-task'])

const title = ref('')
const priority = ref('Medium')
const category = ref('')

const submitTask = () => {
  if (!title.value.trim() || !category.value.trim()) return

  emit('add-task', {
    id: Date.now(),
    title: title.value,
    priority: priority.value,
    category: category.value,
    completed: false
  })

  // Reset form
  title.value = ''
  priority.value = 'Medium'
  category.value = ''
}
</script>

<style scoped>
.card {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  margin-bottom: 20px;
}

.form-group {
  display: flex;
  gap: 10px;
  margin-top: 10px;
}

input, select {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  flex: 1;
}

.btn {
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
}

.btn-primary {
  background: #10b981;
  color: white;
}

.btn-primary:hover {
  background: #059669;
}
</style>