<script setup>
import { ref, computed } from 'vue'
import FormulaireAjout from './components/FormulaireAjout.vue'
import GalerieImages from './components/GalerieImages.vue'

const images = ref([])

let prochainId = 1

const totalImages = computed(() => images.value.length)

const urlsExistantes = computed(() => images.value.map((image) => image.url))

function ajouterImage(url) {
  images.value.push({ id: prochainId++, url })
}

function supprimerImage(id) {
  images.value = images.value.filter((image) => image.id !== id)
}
</script>

<template>
  <main class="galerie-app">
    <h1>Galerie d'images</h1>

    <FormulaireAjout :urls-existantes="urlsExistantes" @ajouter="ajouterImage" />

    <p class="total">Total d'images : {{ totalImages }}</p>

    <GalerieImages :images="images" @supprimer="supprimerImage" />
  </main>
</template>

<style scoped>
.galerie-app {
  max-width: 900px;
  margin: 0 auto;
}

h1 {
  margin-bottom: 1rem;
}

.total {
  margin: 1rem 0;
  font-weight: bold;
}
</style>
