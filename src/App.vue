<script setup lang="ts">
import { computed, ref, watch } from 'vue'
import Bottle from './components/Bottle.vue'
import Harisenbon from './components/Harisenbon.vue'
import { useDeviceMotion, useScreenOrientation } from '@vueuse/core'
import { HARISENBON_WIDTH, BOTTLE_HEIGHT } from './Const'

const { accelerationIncludingGravity, rotationRate } = useDeviceMotion()
const { orientation, isSupported } = useScreenOrientation()

/**
 * ハリセンボンのwrapperのref
 */
const moveRef = ref<HTMLDivElement | null>(null)

/**
 * ボトルの左側のヒビの入った回数
 */
const bottleBottomClashes = ref(0)

/**
 * ボトルの右のヒビの入った回数
 */
 const bottleTopClashes = ref(0)

 /**
  * ゲームがプレイ可能かどうか
  */
 const canPlayGame = computed(() => window.DeviceOrientationEvent && isSupported && accelerationIncludingGravity.value?.x !== null)

/**
 * オリエンテーションが縦かどうか
 */
const isOrientationPortrait = computed(() => orientation.value === 'portrait-secondary' || orientation.value === 'portrait-primary')

/**
 * ハリセンボンのwrapperスタイルのleft
 */
const harisenbonBottomStyle = computed(() => {
  const tilt = isOrientationPortrait ? accelerationIncludingGravity.value?.y : accelerationIncludingGravity.value?.x
  if (!moveRef.value) return BOTTLE_HEIGHT / 2 - HARISENBON_WIDTH / 2
  const currentPosition = parseInt(moveRef.value.style.bottom || '0', 10) || 0
  console.log(currentPosition)
  const newPosition = currentPosition + (tilt || 0) * 1.2
  return Math.max(0, Math.min(BOTTLE_HEIGHT - HARISENBON_WIDTH, newPosition)) // ボトル内に収める
})



watch(harisenbonBottomStyle, () => {
  const { x, y, z } = accelerationIncludingGravity.value || {}
  const { alpha, beta } = rotationRate.value || {}

  // 勢いの閾値を設定します
  const threshold = 15

  // 勢いの計算（回転率の beta を加える）
  const momentum = Math.sqrt(((x || 0) + ((isOrientationPortrait ? beta : alpha) || 0)) ** 2 + (y || 0) ** 2 + (z || 0) ** 2)

  if (momentum > threshold) {
    // ハリセンボンをクラッシュさせる処理を追加します
    if (harisenbonBottomStyle.value === 0) {
      bottleBottomClashes.value++
      // ボトルがクラッシュした状態にするなどの処理を追加します
    }
    if (harisenbonBottomStyle.value === BOTTLE_HEIGHT - HARISENBON_WIDTH) {
      bottleTopClashes.value++
      // ボトルがクラッシュした状態にするなどの処理を追加します
    }
  }
})

</script>

<template>
  <div class="wrapper">
    <dialog class="dialog" :open="!canPlayGame">
      このゲームはお使いのデバイスでは使用できません！
    </dialog>
    <div class="info">
      bottleTopClashes: {{ bottleTopClashes }}<br>
      bottleBottomClashes: {{ bottleBottomClashes }}
    </div>
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
  height: 50px;
}

.dialog {
  z-index: 100;
}
</style>
