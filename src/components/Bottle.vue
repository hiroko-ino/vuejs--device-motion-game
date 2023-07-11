<script setup lang="ts">
defineProps({
  orientation: {
    type: String,
    default: 'portrait'
  }
})
</script>

<template>
  <div class="bottle" :class="{ 'bottle--portrait': orientation === 'portrait', 'bottle--landscape': orientation === 'landscape' }">
    <slot />
  </div>
</template>

<style scoped>
.bottle {
  position: relative;
  background-color: blue;
}

.bottle--portrait {
  width: 300px;
  height: 200px;
}

.bottle--landscape {
  width: 200px;
  height: 300px;
}
</style>
