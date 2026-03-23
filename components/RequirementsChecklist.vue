<script setup lang="ts">
defineProps<{
  checked?: boolean[]
}>()

const requirements = [
  'Needs to run on hardware I already have (Raspberry Pi 3b+)',
  'Require no configuration after installation',
  'Configurable during build time',
  'Testable (at least somewhat)'
]
</script>

<template>
  <div class="w-full max-w-3xl space-y-4">
    <h1>Requirements</h1>
    <div
      v-for="(req, index) in requirements"
      :key="index"
      class="flex items-center justify-between gap-4 text-xl"
    >
      <span :class="{ 'line-through opacity-50': checked?.[index] }">
        {{ req }}
      </span>
      <div
        class="w-6 h-6 border-2 rounded flex items-center justify-center transition-all flex-shrink-0"
        :class="checked?.[index] ? 'bg-green text-black border-green' : 'border-gray-500'"
      >
        <span v-if="checked?.[index]">✓</span>
      </div>
    </div>
  </div>
</template>
