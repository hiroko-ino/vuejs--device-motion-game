<script setup lang="ts">
import { computed, ref } from 'vue'
import Bottle from './components/Bottle.vue'
import Harisenbon from './components/Harisenbon.vue'
import { useDeviceMotion } from '@vueuse/core'
import { HARISENBON_WIDTH, BOTTLE_WIDTH } from './Const'

const { accelerationIncludingGravity } = useDeviceMotion()

/**
 * ハリセンボンのwrapperのref
 */
const moveRef = ref<HTMLDivElement | null>(null)

/**
 * ハリセンボンのwrapperスタイルのleft
 */
const harisenbonLeftStyle = computed(() => {
  const xTilt = accelerationIncludingGravity.value?.x
  if (!moveRef.value) return BOTTLE_WIDTH / 2 - HARISENBON_WIDTH / 2
  const currentPosition = parseInt(moveRef.value.style.left || '0', 10) || 0
  console.log(currentPosition)
  const newPosition = currentPosition + (xTilt || 0)
  return Math.max(0, Math.min(BOTTLE_WIDTH - HARISENBON_WIDTH, newPosition)) // ボトル内に収める
})
</script>

<template>
  <div class="wrapper">
    <Bottle>
      <div ref="moveRef" class="move" :style="{ left: `${harisenbonLeftStyle}px` }">
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
}
</style>
