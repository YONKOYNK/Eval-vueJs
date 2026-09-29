<script setup>
import VignetteImage from './VignetteImage.vue'

defineProps({
  images: {
    type: Array,
    required: true,
  },
})

const emit = defineEmits(['supprimer'])
</script>

<template>
  <p v-if="images.length === 0" class="vide">Aucune image ajoutée</p>

  <ul v-else class="galerie">
    <VignetteImage
      v-for="image in images"
      :key="image.id"
      :image="image"
      @supprimer="(id) => emit('supprimer', id)"
    />
  </ul>
</template>

<style scoped>
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
</style>
