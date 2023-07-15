<script setup lang="ts">
import { computed, ref, watch } from 'vue'
import Bottle from './components/Bottle.vue'
import Harisenbon from './components/Harisenbon.vue'
import { useDeviceMotion, useScreenOrientation } from '@vueuse/core'
import { HARISENBON_WIDTH, BOTTLE_HEIGHT } from './Const'

const { accelerationIncludingGravity } = useDeviceMotion()
const { orientation, isSupported } = useScreenOrientation()

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
  * ゲームがプレイ可能かどうか
  */
 const canPlayGame = computed(() => window.DeviceOrientationEvent && isSupported && accelerationIncludingGravity.value?.x !== null)

/**
 * ハリセンボンのwrapperスタイルのleft
 */
const harisenbonBottomStyle = computed(() => {
  const tilt = orientation.value === 'portrait-secondary' || orientation.value === 'portrait-primary' ? accelerationIncludingGravity.value?.y : accelerationIncludingGravity.value?.x
  if (!moveRef.value) return BOTTLE_HEIGHT / 2 - HARISENBON_WIDTH / 2
  const currentPosition = parseInt(moveRef.value.style.bottom || '0', 10) || 0
  console.log(currentPosition)
  const newPosition = currentPosition + (tilt || 0)
  return Math.max(0, Math.min(BOTTLE_HEIGHT - HARISENBON_WIDTH, newPosition)) // ボトル内に収める
})

watch(harisenbonBottomStyle, () => {
  if (harisenbonBottomStyle.value === 0) {
    bottleLeftClashes.value++
  }
  if (harisenbonBottomStyle.value === BOTTLE_HEIGHT - HARISENBON_WIDTH) {
    bottleRightClashes.value++
  }
})

</script>

<template>
  <div class="wrapper">
    <!-- <dialog class="dialog" :open="!canPlayGame">
      このゲームはお使いのデバイスでは使用できません！
    </dialog> -->
    <Bottle>
      <div ref="moveRef" class="move" :style="{ bottom: `${harisenbonBottomStyle}px` }">
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
  left: calc(50% - 25px + 20px);
}

.dialog {
  z-index: 100;
}
</style>
