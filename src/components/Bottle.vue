<script setup lang="ts"></script>

<template>
  <div class="bottle">
    <slot />
  </div>
</template>

<style scoped>
.bottle {
  position: relative;
  width: 200px;
  height: 300px;
  background-color: blue;
}
</style>
