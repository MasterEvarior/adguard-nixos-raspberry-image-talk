<script setup lang="ts">
const props = defineProps<{
  title?: string
  src: string
  alt?: string
  maxHeight?: string
  justifyCenter?: boolean
}>()
</script>

<template>
  <div :class="['w-full px-16 h-screen flex flex-col items-center overflow-hidden', props.justifyCenter && 'justify-center']">
    <h1 v-if="title" class="mb-4 shrink-0">
      {{ title }}
    </h1>
    <img 
      :src="src" 
      :alt="alt" 
      class="max-w-full object-contain" 
      :style="{ maxHeight: props.maxHeight || '45vh' }" 
    />
  </div>
</template>
