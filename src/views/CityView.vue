<template>

  <div class="">
    <Suspense>
      <AsyncCityView />
      <template #fallback>
        <p>Loading...</p>
      </template>
    </Suspense>
  </div>
</template>

<script setup>
import { useRouter } from "vue-router";
import axios from "axios";
import AsyncCityView from "../components/AsyncCityView.vue";

// const route = useRouter();
</script>
