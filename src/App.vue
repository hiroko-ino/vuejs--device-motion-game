<script setup lang="ts">
import { computed, ref, watch } from 'vue'
import Bottle from './components/Bottle.vue'
import Harisenbon from './components/Harisenbon.vue'
import { useDeviceMotion, useTransition, TransitionPresets } from '@vueuse/core'
import { HARISENBON_WIDTH, BOTTLE_WIDTH } from './Const'

const { accelerationIncludingGravity, rotationRate } = useDeviceMotion()

/**
 * ハリセンボンのwrapperのref
 */
const moveRef = ref<HTMLDivElement | null>(null)

/**
 * ボトルの左側のヒビの入った回数
 */
const bottleLeftClashes = ref(0)

/**
 * ボトルの右のヒビの入った回数
 */
 const bottleRightClashes = ref(0)

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

 /**
  * イージングをかける
  */
const output = useTransition(harisenbonLeftStyle, {
  duration: 10,
  transition: TransitionPresets.linear,
})

watch(harisenbonLeftStyle, () => {
  if (harisenbonLeftStyle.value === 0) {
    bottleLeftClashes.value++
    if (!moveRef.value) return

    const rotationSpeed = rotationRate.value?.beta || 0
    const newPosition = BOTTLE_WIDTH - HARISENBON_WIDTH + rotationSpeed

    moveRef.value.style.left = `${newPosition}px`
  }
  if (harisenbonLeftStyle.value === BOTTLE_WIDTH - HARISENBON_WIDTH) {
    bottleRightClashes.value++
    if (!moveRef.value) return

    const rotationSpeed = rotationRate.value?.beta || 0
    const newPosition = rotationSpeed

    moveRef.value.style.left = `${newPosition}px`
  }
})


</script>

<template>
  <div class="wrapper">
    <div class="info">
      left: {{ bottleLeftClashes }}
      right: {{ bottleRightClashes }}
      harisenbonLeftStyle: {{ harisenbonLeftStyle }}
      output: {{ output }}
      rotationRate.value?.beta: {{ rotationRate?.beta }}
    </div>
    <Bottle>
      <div ref="moveRef" class="move" :style="{ left: `${output}px` }">
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
