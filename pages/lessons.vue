<template>
  <section class="sec">
    <h1>this is lessons</h1>
  </section>
</template>

<script setup lang="ts">
  interface Lesson {
    name: string;
    id: number;
  }
  let mehrdad: number = 36;
</script>

<style scoped>
  .sec {
    margin: 5rem 5rem;
  }
</style>
