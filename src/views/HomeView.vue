<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative"></div>
    <input
      type="text"
      v-model="searchQuery"
      @input="getSearchResults"
      placeholder="Search for a City"
      class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
    />
    <ul
    v-if="mapboxSearchResults"
      class="absolute bg-weather-secondary text-white shadow-md py-2 px-1 top-[170px]"
    >
      <li
        v-for="searchResult in mapboxSearchResults"
        :key="searchResult.id"
        class="py-2 cursor-pointer"
      >
        {{ searchResult.place_name }}
      </li>
    </ul>
  </main>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";

const apiKey =
  "pk.eyJ1IjoicGF0cmlrYXNkYXBzeXMiLCJhIjoiY2xuMzhvOTd2MGdhYTJpcjNldnJsaWxtMSJ9.hDzpqbLtDvpDxFye8pasfw";
const searchQuery = ref("");
const queryTimeout = ref(null);
const mapboxSearchResults = ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      const res = await axios.get(
        `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${apiKey}&types=place`
      );
      mapboxSearchResults.value = res.data.features;
      console.log(mapboxSearchResults.value);
      return;
    }
    mapboxSearchResults.value = null;
  }, 300);
};
</script>
