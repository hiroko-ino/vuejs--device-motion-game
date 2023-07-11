<script setup lang="ts"></script>

<template>
  <div class="bottle">
    <slot />
  </div>
</template>

<style scoped>
.bottle {
  position: relative;
  width: 300px;
  height: 200px;
  background-color: blue;
}
</style>
