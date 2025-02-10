<script setup>
import { computed } from 'vue'
const {
  chartHeight,
  chartWidth,
  scale,
  labelFormater = (tick) => tick,
  numTicks = 10,
} = defineProps(['chartHeight', 'chartWidth', 'scale', 'numTicks', 'labelFormater'])

const ticks = computed(() => {
  return scale.ticks(numTicks).map((tick) => ({
    value: tick,
    position: scale(tick),
    label: labelFormater(tick),
  }))
})
</script>

<template>
  <g stroke="currentColor" fill="currentColor">
    <line x1="0" :x2="chartWidth" :y1="chartHeight" :y2="chartHeight" />

    <template v-for="tick in ticks" :key="tick.value">
      <g :transform="`translate(${tick.position}, ${chartHeight})`">
        <line x1="0" x2="0" y1="0" y2="5" />
        <text x="0" y="17" text-anchor="middle" font-size="12" stroke="none">
          {{ tick.label }}
        </text>
      </g>
    </template>
  </g>
</template>
