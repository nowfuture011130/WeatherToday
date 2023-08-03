<template>
  <div class="flex flex-col min-h-screen font-Roboto bg-weather-p">
    <SiteNavigation />
    <RouterView />
  </div>
</template>

<script setup>
import { RouterView } from "vue-router";
import SiteNavigation from "./components/SiteNavigation.vue";
</script>

<style lang="scss" scoped></style>
