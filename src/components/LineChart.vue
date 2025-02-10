<script setup>
import { computed } from 'vue'
import * as d3 from 'd3'
import LeftAxis from './LeftAxis.vue'
import BottomAxis from './BottomAxis.vue'

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

const parseDate = d3.timeParse('%Y-%m-%d')

console.log('data', data)

const chartWidth = computed(() => width - margin.left - margin.right)
const chartHeight = computed(() => height - margin.top - margin.bottom)

const xScale = computed(() => {
  return d3
    .scaleUtc()
    .domain(d3.extent(data.map((d) => parseDate(`2025-1-${d.day}`))))
    .range([0, chartWidth.value])
    .nice()
})

const yScale = computed(() => {
  return d3
    .scaleLinear()
    .domain([
      d3.min(data.map((d) => d.temperature.min)),
      d3.max(data.map((d) => d.temperature.max)),
    ])
    .range([chartHeight.value, 0])
    .nice()
})

const maxTempLine = computed(() =>
  d3
    .line()
    .x((d) => xScale.value(parseDate(`2025-1-${d.day}`)))
    .y((d) => yScale.value(d.temperature.max)),
)
const minTempLine = computed(() =>
  d3
    .line()
    .x((d) => xScale.value(parseDate(`2025-1-${d.day}`)))
    .y((d) => yScale.value(d.temperature.min)),
)

const maxTempPoints = computed(() => {
  return data.map((d) => {
    const date = parseDate(`2025-1-${d.day}`)
    return {
      date: date,
      x: xScale.value(date),
      y: yScale.value(d.temperature.max),
      value: d.temperature.max,
      toolTipText: `${d3.timeFormat('%b %d')}: ${d.temperature.max}F`,
    }
  })
})

const maxTempLinePath = computed(() => maxTempLine.value(data))
const minTempLinePath = computed(() => minTempLine.value(data))
</script>

<template>
  <div>
    <svg
      :width="width"
      :height="height"
      :viewBox="`0,0,${width},${height}`"
      preserveAspectRatio="xMinYmax meet"
      class="chart"
    >
      <g :transform="`translate(${margin.left}, ${margin.right})`">
        <LeftAxis :chart-height="chartHeight" :num-ticks="5" :scale="yScale" />
        <BottomAxis
          :chart-height="chartHeight"
          :chart-width="chartWidth"
          :scale="xScale"
          :label-formater="d3.timeFormat('%b %d')"
          :num-ticks="10"
        />
        <path fill="none" stroke="red" stroke-width="1.5" :d="maxTempLinePath"></path>
        <path fill="none" stroke="blue" stroke-width="1.5" :d="minTempLinePath"></path>
        <template v-for="point in maxTempPoints" :key="point.date">
          <circle :cx="point.x" :cy="point.y" r="3" fill="red" />
        </template>
      </g>
    </svg>
  </div>
</template>

<style>
.chart {
  max-width: 100%;
}
</style>
