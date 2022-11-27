<template>
  <v-app>
    <div class="app">
      <navigation />
      <NuxtPage></NuxtPage>
    </div>
  </v-app>
</template>
<script setup>
  import navigation from "./components/navigation.vue";
</script>
<style scoped>
  .app {
    font-family: "Lato";
  }
</style>
