<script setup>
import { reactive } from 'vue'

const issue = defineEmits(['place-data'])

const searchTerm = reactive({
  query: '',
  timeout: null,
  results: null
})

const busqueda = () => {
  clearTimeout(searchTerm.timeout)
  searchTerm.timeout = setTimeout(async () => {
    if (searchTerm.query !== '') {
      const res = await fetch(
    `http://api.weatherapi.com/v1/search.json?key={YOUR_API_KEY}=${searchTerm.query}`

      )

      const data = await res.json()
      searchTerm.results = data
      console.log(searchTerm.results)
    } else {
      searchTerm.results = null
    }
  }, 500)
}

const takeWeather = async (id) => {
  const res = await fetch(
    `http://api.weatherapi.com/v1/forecast.json?key={YOUR_API_KEY}=id:${id}&days=3&aqi=yes&alerts=yes`
  )

  const data = await res.json()
  console.log(data)

  issue('place-data', data)

  searchTerm.query = ''
  searchTerm.results = null
}
</script>

<template>
  <div>
    <!-- campo de busqueda -->
    <form>
      <div class="bg-white border border-indigo-600/30 rounded-lg shadow-lg flex items-center w-full max-w-md">  
        <i class="fa-solid fa-magnifying-glass p-2 text-indigo-600"></i>
        <input
          type="text"
          placeholder="Enter a city"
          class="rounded-r-lg p-2 border-0 outline-0 focus:ring-2 focus:ring-indigo-600 ring-inset w-full"
          v-model="searchTerm.query"
          @input="busqueda"
        />
      </div>
    </form>
    <!-- sugerencias de busqueda -->
    <div class="bg-white my-2 rounded-lg shadow-lg">
      <div v-if="searchTerm.results !== null">
        <div v-for="place in searchTerm.results" :key="place.id">
          <button
            @click="takeWeather(place.id)"
            class="px-3 my-2 hover:text-indigo-600 hover:font-bold w-full text-left"
          >
            {{ place.name }}, {{ place.region }}, {{ place.country }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
