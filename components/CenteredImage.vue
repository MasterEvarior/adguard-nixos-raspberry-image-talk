<script setup lang="ts">
defineProps<{
  title?: string
  src: string
  alt?: string
}>()
</script>

<template>
  <div class="flex flex-col items-center justify-center w-full px-16 py-8">
    <h2 v-if="title" class="text-4xl font-bold text-center mb-8">
      {{ title }}
    </h2>
    <img :src="src" :alt="alt" class="max-w-full max-h-[50vh] object-contain" />
  </div>
</template>
