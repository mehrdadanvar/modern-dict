<template>
  <div class="home">
    <div class="cards">
      <v-card title="Dictionary" subtitle="Subtitle" text="..." variant="tonal" class="card">
        <v-card-actions>
          <v-btn>Click me</v-btn>
        </v-card-actions>
      </v-card>
      <v-card title="Lessons" subtitle="Subtitle" text="..." variant="tonal" class="card">
        <v-card-actions>
          <v-btn>Click me</v-btn>
        </v-card-actions>
      </v-card>
      <v-card title="Writing" subtitle="Subtitle" text="..." variant="tonal" class="card">
        <v-card-actions>
          <v-btn>Click me</v-btn>
        </v-card-actions>
      </v-card>
      <v-card title="Card title" subtitle="Subtitle" text="..." variant="tonal" class="card">
        <v-card-actions>
          <v-btn>Click me</v-btn>
        </v-card-actions>
      </v-card>
    </div>
    <dictionary />
  </div>
</template>

<script setup></script>

<style>
  .home {
    margin: 5rem 4rem;
    border: 1px solid green;
  }
  .cards {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
    max-width: 25rem;
  }
  .card {
    background-color: rgb(198, 192, 191);
    border: 1px solid green;
    border-radius: 1rem;
  }
</style>
