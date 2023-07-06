<script setup lang="ts">
import { computed, reactive } from 'vue'
import { useDeviceMotion } from '@vueuse/core'

const motion = reactive(useDeviceMotion())
const tempText = Object.assign({}, motion);
const text = computed(() => JSON.stringify(tempText, null, 2))
</script>

<template>
  <note class="mb-2">
    Device Motion:
  </note>
  <pre lang="json">{{ text }}</pre>
</template>