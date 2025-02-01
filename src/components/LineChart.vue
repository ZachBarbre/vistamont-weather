<script setup>
const props = defineProps({
  data: {
    type: Array,
    required: true,
  },
  width: {
    type: Number,
    default: 400,
  },
  height: {
    type: Number,
    default: 400,
  },
  margin: {
    type: Object,
    default: () => ({
      top: 20,
      right: 20,
      bottom: 30,
      left: 40,
    }),
  },
})

console.log('🚀 ~ props:', props.data)
</script>

<template>
  <svg :width="width" :height="height"></svg>
</template>
