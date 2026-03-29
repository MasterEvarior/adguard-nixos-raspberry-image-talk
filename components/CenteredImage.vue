<script setup lang="ts">
defineProps<{
  title?: string
  src: string
  alt?: string
}>()
</script>

<template>
  <div class="flex flex-col items-center w-full px-16 py-8 h-screen absolute top-0 left-0 right-0">
    <h1 v-if="title" class="mb-4">
      {{ title }}
    </h1>
    <img :src="src" :alt="alt" class="h-[50vh] w-auto object-contain" />
  </div>
</template>
