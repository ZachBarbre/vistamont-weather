<script setup>
import { computed } from 'vue'
const {
  chartHeight,
  scale,
  labelFormater = (tick) => tick,
  numTicks = 10,
} = defineProps(['chartHeight', 'scale', 'numTicks', 'labelFormater'])

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
    <line x1="0" x2="0" y1="0" :y2="chartHeight" stroke="currentColor" />
    <template v-for="tick in ticks" :key="tick.value">
      <g :transform="`translate(0, ${tick.position})`">
        <line x1="0" x2="-5" y1="0" y2="0" />
        <text x="-7" dy="0.23em" text-anchor="end" font-size="12" stroke="none">
          {{ tick.label }}
        </text>
      </g>
    </template>
  </g>
</template>
