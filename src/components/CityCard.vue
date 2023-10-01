<template>
  <div
    class="flex py-6 px-3 bg-weather-secondary rounded-md shadow-md cursor-pointer"
  >
    <div class="flex flex-col flex-1">
      <h2 class="text-3xl">{{ city.city }}</h2>
      <h3>{{ city.state }}</h3>
    </div>
    <div class="flex flex-col gap-2">
      <div class="flex items-center justify-center">
        <p class="text-3xl">{{ Math.round(city.weather.main.temp) }}&deg;</p>
        <img
          :src="`http://openweathermap.org/img/wn/${city.weather.weather[0].icon}@2x.png`"
          alt=""
          class="w-[50px]"
        />
      </div>
      <div class="flex gap-2">
        <span class="text-xs">
          Low: {{ Math.round(city.weather.main.temp_min) }}&deg;
        </span>
        <span class="text-xs">
          High: {{ Math.round(city.weather.main.temp_max) }}&deg;
        </span>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  city: {
    type: Object,
    required: true,
    default: () => {},
  },
});
</script>
