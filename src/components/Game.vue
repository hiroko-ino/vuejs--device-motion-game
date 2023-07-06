<script setup lang="ts">
import { useDeviceMotion } from '@vueuse/core'

const { acceleration, accelerationIncludingGravity, interval, rotationRate } = useDeviceMotion()
</script>

<template>
  <div>
    <p>motion.acceleration?.x： {{ acceleration?.x }}</p>
    <p>motion.acceleration?.y：{{ acceleration?.y }}</p>
    <p>motion.acceleration?.z： {{ acceleration?.z }}</p>
    <p>motion.accelerationIncludingGravity?.x： {{ accelerationIncludingGravity?.x }}</p>
    <p>motion.accelerationIncludingGravity?.y： {{ accelerationIncludingGravity?.y }}</p>
    <p>motion.accelerationIncludingGravity?.z： {{ acceleration?.z }}</p>
    <p>motion.interval： {{ interval }}</p>
    <p>motion.rotationRate?.alpha： {{ rotationRate?.alpha }}</p>
    <p>motion.rotationRate?.gamma： {{ rotationRate?.gamma }}</p>
    <p>motion.rotationRate?.beta： {{ rotationRate?.beta }}</p>
  </div>
</template>