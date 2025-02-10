<script setup>
import { ref } from 'vue'
import data from './assets/january-25-weather.json'
import LineChart from './components/LineChart.vue'

const januaryData = ref(data.january.days)
</script>

<template>
  <header>
    <h1>Vistamont Weather</h1>
  </header>

  <main>
    <LineChart :data="januaryData" width="800" />
    <div>This will a list of the top five coldest and hotest days</div>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }
}
</style>
