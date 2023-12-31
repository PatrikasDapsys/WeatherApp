<template>
  <div class="flex flex-col flex-1 items-center">
    <!-- Banner -->
    <div
      v-if="route.query.preview"
      class="text-white p-4 bg-weather-secondary w-full text-center"
    >
      You are currently previewing this city, click the "+" icon to start
      tracking this city.
    </div>
    <!-- Weather Overview -->
    <div class="flex flex-col items-center text-white py-12">
      <h1 class="text-4xl mb-2">{{ route.params.city }}</h1>
      <p class="text-sm mb-12">
        {{
          new Date(weatherData.currentTime).toLocaleDateString("en-us", {
            weekday: "short",
            day: "2-digit",
            month: "long",
          })
        }}
        {{
          new Date(weatherData.currentTime).toLocaleTimeString("en-us", {
            timeStyle: "short",
          })
        }}
      </p>
      <div class="flex">
        <div class="flex flex-col text-center pr-4 border-r mr-4">
          <p class="text-8xl mb-8">
            {{ Math.round(weatherData.current.temp) }}&deg;
          </p>
          <p>
            Feels like {{ Math.round(weatherData.current.feels_like) }}&deg;
          </p>
        </div>
        <hr />
        <div class="flex flex-col">
          <img
            class="h-[128px]"
            :src="`http://openweathermap.org/img/wn/${weatherData.current.weather[0].icon}@2x.png`"
          />
          <p class="capitalize text-center">
            {{ weatherData.current.weather[0].description }}
          </p>
        </div>
      </div>
    </div>
    <hr class="border-opacity-50 border w-full border-white" />
    <!-- Hourly Weather -->
    <div class="max-w-screen-md w-full py-12">
      <div class="mx-8 text-white">
        <h2 class="mb-4 text-center text-2xl">Hourly Weather</h2>
        <div class="flex gap-10 overflow-x-scroll">
          <div
            v-for="hourData in weatherData.hourly"
            :key="hourData.dt"
            class="flex flex-col gap-4 items-center"
          >
            <p class="text-md whitespace-nowrap">
              {{
                new Date(hourData.currentTime).toLocaleTimeString("en-GB", {
                  hour: "numeric",
                })
              }}h
            </p>
            <img
              class="w-auto h-[50px] object-cover"
              :src="`http://openweathermap.org/img/wn/${hourData.weather[0].icon}@2x.png`"
            />
            <p>{{ Math.round(hourData.temp) }}&deg;</p>
          </div>
        </div>
      </div>
    </div>
    <hr class="border-opacity-50 border w-full border-white" />
    <!-- Weekly Weather -->
    <div class="max-w-screen-md w-full py-12">
      <div class="mx-8 text-white">
        <h2 class="mb-4 text-center text-2xl">7 Day Forecast</h2>
        <div
          v-for="day in weatherData.daily"
          :key="day.dt"
          class="flex items-center"
        >
          <p class="flex-1">
            {{
              new Date(day.dt * 1000).toLocaleDateString("en-us", {
                weekday: "long",
              })
            }}
          </p>
          <img
            class="w-[50px] h-[50px] object-cover"
            :src="`http://openweathermap.org/img/wn/${day.weather[0].icon}@2x.png`"
          />
          <div class="flex gap-2 flex-1 justify-end">
            <p>Low: {{ Math.round(day.temp.min) }}&deg;</p>
            <p>High: {{ Math.round(day.temp.max) }}&deg;</p>
          </div>
        </div>
      </div>
    </div>

    <div
      @click="removeCity()"
      class="flex items-center gap-2 py-12 text-white cursor-pointer duration-150 hover:text-red-500"
    >
      <i class="fa-solid fa-trash"></i>
      <p>Remove City</p>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { useRoute, useRouter } from "vue-router";

const route = useRoute();
const router = useRouter();
const getWeatherData = async () => {
  try {
    const weatherData = await axios.get(
      `https://api.openweathermap.org/data/2.5/onecall?lat=${route.query.lat}&lon=${route.query.lng}&appid=7efa332cf48aeb9d2d391a51027f1a71&units=metric`
    );
    const localOffset = new Date().getTimezoneOffset() * 60000;
    const utc = weatherData.data.current.dt * 1000 + localOffset;
    weatherData.data.currentTime =
      utc + 1000 * weatherData.data.timezone_offset;

    // cal hourly weather offset
    weatherData.data.hourly.forEach((hour) => {
      const utc = hour.dt * 1000 + localOffset;
      hour.currentTime = utc + 1000 * weatherData.data.timezone_offset;
    });
    return weatherData.data;
  } catch (error) {
    console.log(error);
  }
};
const weatherData = await getWeatherData();

const removeCity = () => {
  const cities = JSON.parse(localStorage.getItem("saved-cities"));
  const updatedCities = cities.filter((city) => city.id !== route.query.id);
  localStorage.setItem("saved-cities", JSON.stringify(updatedCities));
  router.push({ name: "home" });
};
</script>
