<script setup>
import { ref } from 'vue'

const nouvelleUrl = ref('')
</script>

<template>
  <main class="galerie-app">
    <h1>Galerie d'images</h1>

    <form class="formulaire">
      <input
        v-model="nouvelleUrl"
        type="text"
        placeholder="URL de l'image (https://...)"
        aria-label="URL de l'image"
      />
      <button type="submit">Ajouter</button>
    </form>
  </main>
</template>
