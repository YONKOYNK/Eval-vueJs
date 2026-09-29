<script setup>
import { ref, computed } from 'vue'

const nouvelleUrl = ref('')

const images = ref([])

let prochainId = 1

const totalImages = computed(() => images.value.length)

function ajouterImage() {
  const url = nouvelleUrl.value.trim()

  images.value.push({ id: prochainId++, url })
  nouvelleUrl.value = ''
}

function supprimerImage(id) {
  images.value = images.value.filter((image) => image.id !== id)
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

    <p class="total">Total d'images : {{ totalImages }}</p>
    <p v-if="totalImages === 0" class="vide">Aucune image ajoutée</p>

    <ul v-else class="galerie">
      <li v-for="image in images" :key="image.id" class="vignette">
        <img :src="image.url" alt="Image de la galerie" />
        <button class="supprimer" @click="supprimerImage(image.id)">Supprimer</button>
      </li>
    </ul>
  </main>
</template>
