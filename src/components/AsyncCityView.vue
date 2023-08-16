<template>
  <div class="flex flex-col flex-1 items-center">
    <!-- 只在我们在预览城市时才会显示的提示 -->
    <div
      v-if="route.query.preview"
      class="text-white p-4 bg-weather-s w-full text-center"
    >
      <p>
        You are currently previewing this city, chick "+" icon to start tracking
        this city.
      </p>
    </div>
    <!-- 显示所有的天气情况 -->
    <div class="flex flex-col items-center text-white py-12">
      <!-- 显示城市名 -->
      <h1 class="text-4xl mb-2">{{ route.params.city }}</h1>
      <!-- 显示时间 -->
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
      <!-- 显示现在温度 -->
      <p class="text-8xl mb-8">
        {{ Math.round(weatherData.current.temp) }}&deg;
      </p>
      <!-- 显示现在的体感温度 -->
      <p>
        Feels like
        {{ Math.round(weatherData.current.feels_like) }} &deg;
      </p>
      <!-- 显示现在的天气 -->
      <p class="capitalize">
        {{ weatherData.current.weather[0].description }}
      </p>
      <!-- 显示现在天气图片 -->
      <img
        class="w-[150px] h-auto"
        :src="`http://openweathermap.org/img/wn/${weatherData.current.weather[0].icon}@2x.png`"
        alt=""
      />
    </div>

    <!-- 一条分割线 -->
    <hr class="border-white border-opacity-10 border w-full" />

    <!-- 每小时温度 -->
    <div class="max-w-screen-md w-full py-12">
      <div class="mx-8 text-white">
        <!-- 标题 -->
        <h2 class="mb-4">Hourly Weather</h2>
        <!-- 创建一个可滚动的容器 -->
        <div class="flex gap-10 overflow-x-scroll">
          <div
            v-for="hourData in weatherData.hourly"
            :key="hourData.dt"
            class="flex flex-col gap-4 items-center"
          >
            <p class="whitespace-nowrap text-md">
              {{
                new Date(hourData.currentTime).toLocaleTimeString("en-us", {
                  hour: "numeric",
                })
              }}
            </p>

            <img
              class="w-auto h-[50px] object-cover"
              :src="`http://openweathermap.org/img/wn/${hourData.weather[0].icon}@2x.png`"
              alt=""
            />
            <p class="text-xl">{{ Math.round(hourData.temp) }}&deg;</p>
          </div>
        </div>
      </div>
    </div>

    <!-- 分割线 -->
    <hr class="border-white border-opacity-10 border w-full" />

    <!-- 每周天气 -->
    <div class="max-w-screen-md w-full py-12">
      <div class="mx-8 text-white">
        <h2 class="mb-4">7 Day Forecast</h2>
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
            alt=""
          />
          <div class="flex gap-2 flex-1 justify-end">
            <p>H: {{ Math.round(day.temp.max) }}&deg;</p>
            <p>L: {{ Math.round(day.temp.min) }}&deg;</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { useRoute } from "vue-router";
const route = useRoute();
//获得现在天气信息
const getWeatherData = async () => {
  try {
    const weatherData = await axios.get(
      `https://api.openweathermap.org/data/3.0/onecall?lat=${route.query.lat}&lon=${route.query.lng}&exclude={part}&appid=e6a0bdab1b851861432d8a02f69d6ba3`
    );
    //计算现在的日期和时间
    const localOffset = new Date().getTimezoneOffset() * 60000;
    const utc = weatherData.data.current.dt * 1000 + localOffset;
    weatherData.data.currentTime =
      utc + 1000 * weatherData.data.timezone_offset;

    //计算真实的每小时天气的时间
    weatherData.data.hourly.forEach((hour) => {
      const utc = hour.dt * 1000 + localOffset;
      hour.currentTime = utc + 1000 * weatherData.data.timezone_offset;
    });

    //将获得的数据返回
    return weatherData.data;
  } catch (err) {
    console.log(err);
  }
};
//需要执行getWeatherData来获得数据，之后weatherData就可以随意使用了
const weatherData = await getWeatherData();
</script>
