<template>
  <div class="overlay">
    <div class="card danger">
      <button class="btn close" @click="close">
        <img src="/src/icons/Close.svg" alt="close" />
      </button>
      <strong><h2>Неподходящий размер или формат файла</h2></strong>
      <h3>Размер файла не должен превышать 5мб.</h3>
      <h3>Допустимые форматы файла: jpg, jpeg, png, gif, webp.</h3>
    </div>
  </div>
</template>

<script setup>
import { inject } from 'vue'

let errorImg = inject('errorImg')
function close() {
  return (errorImg.value = false)
}
</script>

<style scoped>
.danger {
  z-index: 2;
  text-align: center;
}

.card {
  width: auto;
  height: auto;
}

.btn.close {
  margin-left: 97%;
}
</style>
