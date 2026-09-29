<script setup>
defineProps({
  image: {
    type: Object,
    required: true,
  },
})

const emit = defineEmits(['supprimer'])
</script>

<template>
  <li class="vignette">
    <img :src="image.url" alt="Image de la galerie" />
    <button class="supprimer" @click="emit('supprimer', image.id)">Supprimer</button>
  </li>
</template>

<style scoped>
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
