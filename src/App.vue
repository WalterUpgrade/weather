<script setup>
import { ref } from 'vue'
import SearchCountry from './components/SearchCountry.vue'
import WeatherCard from './components/WeatherCard.vue'

const cities = ref([])

const Lugares = (data) => {
  cities.value.push(data)
}

const deleteLugares = (name) => {
  if (confirm('Are you sure')) {
    cities.value = cities.value.filter((p) => p.location.name !== name)
  }
}
</script>
<template>
  <main>
    <!-- Fecha -->
    <div class="text-center mb-6">
  <span class="text-white mr-2">{{ 
    new Date().toLocaleDateString('en-us', { weekday: 'long' }) 
  }}</span>
  <span class="text-gray-400">{{
    new Date().toLocaleDateString('en-us', { month: 'long', day: 'numeric', year: 'numeric' })
  }}</span>
</div>

    <!-- Busqueda -->
    <div>
      <SearchCountry @place-data="Lugares" />
    </div>

    <!-- Tarjeta Meteorologica -->
    <div class="grid grid-cols-1 mt-10 sm:grid-cols-2 gap-4">
      <div v-for="(place, idx) in cities" :key="idx">
        <WeatherCard :place="place" @delete-place="deleteLugares" />
      </div>
    </div>
  </main>
</template>
