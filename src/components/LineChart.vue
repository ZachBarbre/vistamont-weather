<script setup>
import { computed } from 'vue'
import * as d3 from 'd3'

const {
  data,
  width = 400,
  height = 400,
  margin = {
    top: 20,
    right: 20,
    bottom: 30,
    left: 40,
  },
} = defineProps(['data', 'width', 'height', 'margin'])

console.log('data', data)

const chartWidth = computed(() => width - margin.left - margin.right)
const chartHeight = computed(() => height - margin.top - margin.bottom)

const xScale = computed(() => {
  return d3
    .scaleUtc()
    .domain(d3.extent(data.map((d) => new Date(2025, 1, d.day))))
    .range([0, chartWidth.value])
})
const yScale = computed(() => {
  return d3
    .scaleLinear()
    .domain([
      d3.min(data.map((d) => d.temperature.min)),
      d3.max(data.map((d) => d.temperature.max)),
    ])
    .range([chartHeight.value, 0])
})

const maxTempLine = computed(() =>
  d3
    .line()
    .x((d) => xScale.value(new Date(2025, 1, d.day)))
    .y((d) => yScale.value(d.temperature.max)),
)
const minTempLine = computed(() =>
  d3
    .line()
    .x((d) => xScale.value(new Date(2025, 1, d.day)))
    .y((d) => yScale.value(d.temperature.min)),
)

const maxTempLinePath = computed(() => maxTempLine.value(data))
const minTempLinePath = computed(() => minTempLine.value(data))
</script>

<template>
  <svg :width="width" :height="height">
    <path fill="none" stroke="red" stroke-width="1.5" :d="maxTempLinePath"></path>
    <path fill="none" stroke="blue" stroke-width="1.5" :d="minTempLinePath"></path>
  </svg>
</template>
