<template>
  <div class="">
    <Suspense>
      <AsyncCityView />
      <template #fallback>
        <CityViewSkeleton />
      </template>
    </Suspense>
  </div>
</template>

<script setup>
import { useRouter } from "vue-router";
import axios from "axios";
import AsyncCityView from "../components/AsyncCityView.vue";
import CityViewSkeleton from "../components/CityViewSkeleton.vue";

// const route = useRouter();
</script>
