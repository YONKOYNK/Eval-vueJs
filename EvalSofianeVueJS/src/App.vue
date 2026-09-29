<script setup>
import { ref, computed } from 'vue'

const nouvelleUrl = ref('')

const images = ref([])

const erreur = ref('')

const chargement = ref(false)

let prochainId = 1

const totalImages = computed(() => images.value.length)

function estUrlValide(texte) {
  try {
    const url = new URL(texte)
    return url.protocol === 'http:' || url.protocol === 'https:'
  } catch {
    return false
  }
}

function chargerImage(url) {
  return new Promise((resolve) => {
    const img = new Image()
    const delai = setTimeout(() => resolve(false), 8000)

    img.onload = () => {
      clearTimeout(delai)
      resolve(img.naturalWidth > 0)
    }
    img.onerror = () => {
      clearTimeout(delai)
      resolve(false)
    }
    img.src = url
  })
}

async function ajouterImage() {
  if (chargement.value) return

  const url = nouvelleUrl.value.trim()

  if (url === '') {
    erreur.value = 'Veuillez saisir une URL.'
    return
  }

  if (!estUrlValide(url)) {
    erreur.value = "L'URL saisie n'est pas valide (elle doit commencer par http:// ou https://)."
    return
  }

  if (images.value.some((image) => image.url === url)) {
    erreur.value = 'Cette image est déjà dans la galerie.'
    return
  }

  erreur.value = ''
  chargement.value = true
  const estUneImage = await chargerImage(url)
  chargement.value = false

  if (!estUneImage) {
    erreur.value = "Aucune image n'a pu être chargée à cette adresse."
    return
  }

  images.value.push({ id: prochainId++, url })
  nouvelleUrl.value = ''
  erreur.value = ''
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
      <button type="submit" :disabled="chargement">
        {{ chargement ? 'Vérification...' : 'Ajouter' }}
      </button>
    </form>

    <p v-if="erreur" class="erreur">{{ erreur }}</p>

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

<style scoped>
.galerie-app {
  max-width: 900px;
  margin: 0 auto;
}

h1 {
  margin-bottom: 1rem;
}

.formulaire {
  display: flex;
  gap: 0.5rem;
}

.formulaire input {
  flex: 1;
  padding: 0.5rem;
  font-size: 1rem;
}

button {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  cursor: pointer;
}

button:disabled {
  cursor: wait;
  opacity: 0.6;
}

.erreur {
  color: #d33;
  margin-top: 0.5rem;
}

.total {
  margin: 1rem 0;
  font-weight: bold;
}

.vide {
  font-style: italic;
  color: #888;
}

.galerie {
  list-style: none;
  padding: 0;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
  gap: 1rem;
}

.vignette {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
}

.vignette img {
  width: 100%;
  height: 150px;
  object-fit: cover;
  border-radius: 6px;
  border: 1px solid #ccc;
}

.supprimer {
  width: 100%;
}
</style>
