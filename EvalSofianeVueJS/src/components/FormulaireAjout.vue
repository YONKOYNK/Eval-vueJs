<script setup>
import { ref } from 'vue'

const props = defineProps({
  urlsExistantes: {
    type: Array,
    default: () => [],
  },
})

const emit = defineEmits(['ajouter'])

const nouvelleUrl = ref('')

const erreur = ref('')

const chargement = ref(false)

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

async function valider() {
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

  if (props.urlsExistantes.includes(url)) {
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

  emit('ajouter', url)
  nouvelleUrl.value = ''
  erreur.value = ''
}
</script>

<template>
  <div>
    <form class="formulaire" @submit.prevent="valider">
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
  </div>
</template>

<style scoped>
.formulaire {
  display: flex;
  gap: 0.5rem;
}

.formulaire input {
  flex: 1;
  padding: 0.5rem;
  font-size: 1rem;
}

.erreur {
  color: #d33;
  margin-top: 0.5rem;
}
</style>
