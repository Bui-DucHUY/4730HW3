<template>
  <div class="stats-grid card">
    <div class="stat-box">
      <h3>Total Tasks</h3>
      <p>{{ total }}</p>
    </div>
    <div class="stat-box">
      <h3>Active</h3>
      <p>{{ active }}</p>
    </div>
    <div class="stat-box">
      <h3>Completed</h3>
      <p>{{ completed }}</p>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  tasks: {
    type: Array,
    required: true
  }
})

const total = computed(() => props.tasks.length)
const completed = computed(() => props.tasks.filter(t => t.completed).length)
const active = computed(() => props.tasks.filter(t => !t.completed).length)
</script>

<style scoped>
.stats-grid {
  display: flex;
  gap: 20px;
  margin-bottom: 20px;
}

.stat-box {
  flex: 1;
  text-align: center;
  background: #f8fafc;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
}

.stat-box h3 { margin: 0 0 5px 0; color: #64748b; font-size: 1rem; }
.stat-box p { margin: 0; font-size: 1.5rem; font-weight: bold; color: #0f172a; }
</style>