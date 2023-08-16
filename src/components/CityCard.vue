<template>
  <div class="flex py-6 px-3 bg-weather-s rounded-md shadow-md cursor-pointer">
    <!-- 显示城市名字 -->
    <div class="flex flex-col flex-1">
      <h2 class="text-3xl">{{ city.city }}</h2>
      <h3>{{ city.state }}</h3>
    </div>
    <!-- 显示城市温度 -->
    <div class="flex flex-col gap-2">
      <p class="text-3xl self-end">
        {{ Math.round(city.weather.main.temp) }}&deg;
      </p>

      <div class="flex gap-2">
        <span class="text-xs">
          H:
          {{ Math.round(city.weather.main.temp_max) }}&deg;
        </span>
        <span class="text-xs">
          L:
          {{ Math.round(city.weather.main.temp_min) }}&deg;
        </span>
      </div>
    </div>
  </div>
</template>

<script setup>
//接收来自CityList的city变量
defineProps({
  city: {
    type: Object,
    default: () => {},
  },
});
</script>
