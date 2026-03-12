<script setup>
import { computed } from 'vue'

const props = defineProps({
  clicks: {
    type: Number,
    required: true
  },
  steps: {
    type: Array,
    required: true
    // Expected format: [{ click: 0, name: 'file1.js' }, { click: 6, name: 'file2.js' }]
  }
})

// Find the correct filename by looking for the highest click threshold we've passed
const currentFile = computed(() => {
  const activeStep = [...props.steps]
    .sort((a, b) => b.click - a.click) // Sort highest click to lowest
    .find(step => props.clicks >= step.click)
    
  return activeStep ? activeStep.name : props.steps[0].name
})
</script>

<template>
  <div class="font-mono text-sm opacity-70 mb-2 bg-[#282a36] inline-block px-4 py-1 rounded-t-md border-b border-[#44475a] transition-all">
    {{ currentFile }}
  </div>
</template>