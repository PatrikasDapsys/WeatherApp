<template>
  <!-- <div class="flex flex-col flex-1 items-center">
    <div
      v-if="route.query.preview"
      class="text-white p-4 bg-weather-secondary w-full text-center"
    >
      You are currently previewing this city, click the "+" icon to start
      tracking this city.
    </div>
  </div> -->
  <div class="">
    <AsyncCityView />
  </div>
</template>

<script setup>
import { useRouter } from "vue-router";
import axios from "axios";
import AsyncCityView from "../components/AsyncCityView.vue";

// const route = useRouter();
</script>
