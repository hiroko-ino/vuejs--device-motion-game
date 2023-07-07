<script setup lang="ts">
import Bottle from './components/Bottle.vue'
import Harisenbon from './components/Harisenbon.vue'
</script>

<template>
  <div class="wrapper">
    <Bottle>
      <div class="move">
        <Harisenbon />
      </div>
    </Bottle>
  </div>
</template>

<style scoped>
.wrapper {
  display: grid;
  place-items: center;
}

.move {
  position: absolute;
  top: calc(50% - 25px);
  left: calc(50% - 25px);
}
</style>
