<template>
  <div>
    <TaskStats :tasks="tasks" />
    <TaskForm @add-task="handleAddTask" />

    <div class="controls card">
      <input 
        v-model="searchQuery" 
        type="text" 
        placeholder="Search tasks..." 
        class="search-bar"
      />
      <select v-model="filterStatus" class="filter-select">
        <option value="All">All Statuses</option>
        <option value="Active">Active Only</option>
        <option value="Completed">Completed Only</option>
      </select>
    </div>

    <div class="task-list">
      <p v-if="filteredTasks.length === 0" class="empty-state">No tasks found.</p>
      
      <TaskItem 
        v-for="task in filteredTasks" 
        :key="task.id" 
        :task="task"
        @toggle-status="handleToggleStatus"
        @delete-task="handleDeleteTask"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch, onMounted } from 'vue'
import TaskForm from '../components/TaskForm.vue'
import TaskItem from '../components/TaskItem.vue'
import TaskStats from '../components/TaskStats.vue'

// State
const tasks = ref([])
const searchQuery = ref('')
const filterStatus = ref('All')

// Load from LocalStorage on mount
onMounted(() => {
  const savedTasks = localStorage.getItem('taskmaster-tasks')
  if (savedTasks) {
    tasks.value = JSON.parse(savedTasks)
  }
})

// Watcher: Save to LocalStorage automatically when tasks array changes deeply
watch(tasks, (newTasks) => {
  localStorage.setItem('taskmaster-tasks', JSON.stringify(newTasks))
}, { deep: true })

// Computed: Filter and Search
const filteredTasks = computed(() => {
  return tasks.value.filter(task => {
    // 1. Search filter
    const matchesSearch = task.title.toLowerCase().includes(searchQuery.value.toLowerCase())
    
    // 2. Status filter
    let matchesStatus = true
    if (filterStatus.value === 'Active') matchesStatus = !task.completed
    if (filterStatus.value === 'Completed') matchesStatus = task.completed

    return matchesSearch && matchesStatus
  })
})

// Actions
const handleAddTask = (newTask) => {
  tasks.value.unshift(newTask)
}

const handleDeleteTask = (taskId) => {
  tasks.value = tasks.value.filter(t => t.id !== taskId)
}

const handleToggleStatus = (taskId) => {
  const task = tasks.value.find(t => t.id === taskId)
  if (task) {
    task.completed = !task.completed
  }
}
</script>

<style scoped>
.controls {
  display: flex;
  gap: 15px;
  margin-bottom: 20px;
  padding: 15px;
  background: white;
}

.search-bar, .filter-select {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.search-bar { flex: 2; }
.filter-select { flex: 1; }

.empty-state {
  text-align: center;
  color: #6b7280;
  padding: 20px;
  background: white;
  border-radius: 8px;
}
</style>