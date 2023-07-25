<script setup>
import { ref, reactive } from 'vue'
const joke = reactive({
  value: 'Chuck Norris can do a wheelie on a unicycle'
})

function getAJoke() {
  // joke.value = "New Joke"
  fetch('https://api.chucknorris.io/jokes/random')
    .then(response => response.json())
    .then(data => {
      joke.value = data.value
    })
}

const countries = reactive([
  { name: "Bangladesh", capital: "Dhaka" },
  { name: "Bhutan", capital: "Thimpu" }
])

function getAllCountries(list = 'All') {
  countries.length = 0
  fetch('https://restcountries.com/v3.1/all?fields=name,capital')
    .then(response => response.json())
    .then(data => {
      if ('All' == list) {
        data.forEach(country => {
          countries.push({
            name: country.name.common,
            capital: country.capital[0]
          })
          
        })
      } else {
        data.filter(c => c.name.common.startsWith(list)).forEach(country => {
          countries.push({
            name: country.name.common,
            capital: country.capital[0]
          })
        })
      }

    })
}

</script>

<template>
  <section class="flex flex-col items-center w-[500px]">
    <p>
      {{ joke.value }}
    </p>
    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mt-5 w-48" @click="getAJoke()">
      Get a new joke
    </button>
  </section>
  <section class="container mx-auto">
    <h1 class="text-2xl">Country List</h1>
    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mt-5 w-48"
      @click="getAllCountries()">
      List of countries
    </button>
    <button class="w-10 ml-2 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mt-5"
      @click="getAllCountries('A')">
      A
    </button>
    <button class="w-10 ml-2 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mt-5"
      @click="getAllCountries('B')">
      B
    </button>
    <button class="w-10 ml-2 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mt-5"
      @click="getAllCountries('C')">
      C
    </button>
    <ul class="mt-10">
      <li v-for="country in countries" :key="country.name">{{ country.name }} = {{ country.capital }}</li>
    </ul>
  </section>
</template>

<style scoped></style>
