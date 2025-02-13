<script setup>
import { ref } from 'vue'
import SearchCountry from './components/SearchCountry.vue'
import WeatherCard from './components/WeatherCard.vue'

const places = ref([])

const addPlace = (data) => {
  places.value.push(data)
}

const deletePlace = (name) => {
  if (confirm('Are you sure')) {
    places.value = places.value.filter((p) => p.location.name !== name)
  }
}
</script>
<template>
  <main>
    <!-- Date -->
    <div class="text-center mb-6">
  <span class="text-white mr-2">{{ 
    new Date().toLocaleDateString('en-us', { weekday: 'long' }) 
  }}</span>
  <span class="text-gray-400">{{
    new Date().toLocaleDateString('en-us', { month: 'long', day: 'numeric', year: 'numeric' })
  }}</span>
</div>

    <!-- Search -->
    <div>
      <SearchCountry @place-data="addPlace" />
    </div>

    <!-- Weather cards -->
    <div class="grid grid-cols-1 mt-10 sm:grid-cols-2 gap-4">
      <div v-for="(place, idx) in places" :key="idx">
        <WeatherCard :place="place" @delete-place="deletePlace" />
      </div>
    </div>
  </main>
</template>
