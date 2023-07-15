<script setup lang="ts">
import Bottle from '../../public/bottle.png'
</script>

<template>
  <div class="bottle">
    <img :src="Bottle" class="bottle__base" alt="ボトル">
    <slot />
  </div>
</template>

<style scoped>
.bottle {
  position: relative;
  width: 320px;
  height: 150px;
}

.bottle__base {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>
