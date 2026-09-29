<script setup>
import { ref } from 'vue'

const nouvelleUrl = ref('')

const images = ref([])

let prochainId = 1

function ajouterImage() {
  const url = nouvelleUrl.value.trim()

  images.value.push({ id: prochainId++, url })
  nouvelleUrl.value = ''
}
</script>

<template>
  <main class="galerie-app">
    <h1>Galerie d'images</h1>

    <form class="formulaire" @submit.prevent="ajouterImage">
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
