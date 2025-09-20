<template>
  <div class="overlay">
    <button class="btn close" @click="$emit('close')">
      <img src="/src/icons/Close.svg" alt="close" />
    </button>
    <div class="image-wrapper">
      <img :src="imgURL" alt="fullImg" />
    </div>
  </div>
</template>

<script setup>
defineProps(['imgURL'])
defineEmits(['close'])
</script>

<style scoped>
.image-wrapper {
  max-width: 90vw;
  max-height: 90vh;
  position: relative;
  display: inline-block;
  overflow: auto;
}

.btn.close {
  position: absolute;
  z-index: 10;
  top: calc(50% - 45vh + 10px);
  right: calc(50% - 45vw + 10px);
  background-color: red;
}

img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  display: block;
}
</style>
