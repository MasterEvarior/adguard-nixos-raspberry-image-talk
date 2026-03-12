<template>
  <div class="flex flex-col justify-center items-center w-full">
    <div class="flex justify-center items-center text-6xl text-center">
      <slot />
    </div>
    
    <br>
    
    <div class="flex justify-center items-center text-3xl text-center">
      <slot name="author" />
    </div>
  </div>
</template>