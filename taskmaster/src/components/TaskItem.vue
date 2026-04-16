<template>
  <div class="task-item card" :class="{ 'is-completed': task.completed }">
    <div class="task-info">
      <input 
        type="checkbox" 
        :checked="task.completed" 
        @change="$emit('toggle-status', task.id)" 
      />
      <div class="task-details">
        <h3 class="task-title">{{ task.title }}</h3>
        <div class="badges">
          <span class="badge category">{{ task.category }}</span>
          <span class="badge priority" :class="task.priority.toLowerCase()">{{ task.priority }}</span>
        </div>
      </div>
    </div>

    <div class="task-actions">
      <div class="timer-display" v-if="timeLeft !== 1500 || timerRunning">
        {{ formatTime }}
      </div>
      <button v-if="!task.completed && !timerRunning" @click="startFocusMode" class="btn btn-focus">Focus</button>
      <button v-if="timerRunning" @click="pauseFocusMode" class="btn btn-warning">Pause</button>
      <button @click="$emit('delete-task', task.id)" class="btn btn-danger">Delete</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  task: {
    type: Object,
    required: true
  }
})

defineEmits(['toggle-status', 'delete-task'])

// Timer
const timeLeft = ref(1500)
const timerRunning = ref(false)
let timerId = null

const formatTime = computed(() => {
  const minutes = Math.floor(timeLeft.value / 60)
  const seconds = timeLeft.value % 60
  return `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`
})

const startFocusMode = () => {
  if (timerRunning.value) return
  timerRunning.value = true
  timerId = setInterval(() => {
    if (timeLeft.value > 0) {
      timeLeft.value--
    } else {
      pauseFocusMode()
      alert(`Focus session for "${props.task.title}" is complete!`)
    }
  }, 1000)
}

const pauseFocusMode = () => {
  timerRunning.value = false
  clearInterval(timerId)
}
</script>

<style scoped>
.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 20px;
  margin-bottom: 10px;
  transition: all 0.2s ease;
}

.is-completed {
  opacity: 0.6;
}

.is-completed .task-title {
  text-decoration: line-through;
  color: #6b7280;
}

.task-info {
  display: flex;
  align-items: center;
  gap: 15px;
}

.task-title {
  margin: 0 0 5px 0;
  font-size: 1.1rem;
}

.badges {
  display: flex;
  gap: 8px;
}

.badge {
  font-size: 0.75rem;
  padding: 3px 8px;
  border-radius: 12px;
  font-weight: bold;
}

.category { background: #e5e7eb; color: #374151; }
.priority.high { background: #fee2e2; color: #ef4444; }
.priority.medium { background: #fef3c7; color: #f59e0b; }
.priority.low { background: #d1fae5; color: #10b981; }

.task-actions {
  display: flex;
  align-items: center;
  gap: 10px;
}

.timer-display {
  font-family: monospace;
  font-size: 1.2rem;
  font-weight: bold;
  color: #2563eb;
}

.btn { padding: 6px 12px; border: none; border-radius: 4px; cursor: pointer; font-weight: bold; }
.btn-focus { background: #3b82f6; color: white; }
.btn-warning { background: #f59e0b; color: white; }
.btn-danger { background: #ef4444; color: white; }
</style>