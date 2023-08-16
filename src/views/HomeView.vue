<template>
  <main class="container text-white">
    <!-- 整个搜索框组件 -->
    <div class="pt-4 mb-8 relative">
      <!-- 搜索框本身 -->
      <input
        type="text"
        v-model="searchQuery"
        @input="getSearchResult"
        placeholder="Search for a city or state"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-s focus:outline-none focus: shadow-[0px_1px_0_0_#004E71]"
      />
      <!-- 在搜索框下面显示搜索内容，只在有搜索内容时显示 -->
      <ul
        class="absolute bg-weather-s text-white w-full shadow-md py-2 px-1 top-[66px]"
        v-if="mapboxSearchResult"
      >
        <!-- 特殊情况字符展示 -->
        <p v-if="searchError">Sorry, something went wrong, please try again.</p>
        <p v-if="!searchError && mapboxSearchResult.length === 0">
          No result found.
        </p>
        <!-- 由于无法在 -->
        <template v-else>
          <!-- 将mapboxSearchResult里的所有搜索内容都展示出来 -->
          <li
            v-for="searchResult in mapboxSearchResult"
            :key="searchResult.id"
            class="py-2 cursor-pointer"
            @click="previewCity(searchResult)"
          >
            {{ searchResult.place_name }}
          </li>
        </template>
      </ul>
    </div>
    <!-- 在搜索栏下面显示所有的内存中的城市 -->
    <div class="flex flex-col gap-4">
      <!-- 由于CityList使用了await， 需要suspense -->
      <Suspense>
        <CityList />
        <template #fallback>
          <CityCardSkeleton />
        </template>
      </Suspense>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";
import CityList from "../components/CityList.vue";
import CityCardSkeleton from "../components/CityCardSkeleton.vue";
const router = useRouter();
const mapboxAPIKey =
  "pk.eyJ1Ijoibm93LWZ1dHVyZSIsImEiOiJjbGt2cHJqcjUwZGNyM2VuMXBlN3Zvc29hIn0.9fjTWMShBnoM_P44sro1dw";
const searchQuery = ref(""); // 为搜索内容
const queryTimeout = ref(null); //为计时器
const mapboxSearchResult = ref(null); //为搜索结果
const searchError = ref(null); //是否有搜索错误
//点击搜索结果时触发
const previewCity = (searchResult) => {
  const [city, state] = searchResult.place_name.split(",");
  //跳转到cityView界面
  router.push({
    name: "cityView",
    params: { state: state.replaceAll(" ", ""), city: city },
    // preview代表着是否是在预览城市天气。如果为true则代表为预览
    query: {
      lat: searchResult.geometry.coordinates[1],
      lng: searchResult.geometry.coordinates[0],
      preview: true,
    },
  });
};
// 当用户在输入框中输入文字后执行函数
const getSearchResult = () => {
  //清除计时器，让其重新计时
  clearTimeout(queryTimeout.value);
  //如果输入后等待一段时间触发回调
  queryTimeout.value = setTimeout(async () => {
    //判断输入内容不为空并进行地区搜索
    if (searchQuery.value !== "") {
      try {
        const result = await axios.get(
          `https://api.mapbox.com/geocoding/v5/mapbox.places/${searchQuery.value}.json?access_token=${mapboxAPIKey}&type=place`
        );
        mapboxSearchResult.value = result.data.features;
      } catch {
        searchError.value = true;
      }

      return;
    }
    mapboxSearchResult.value = null;
  }, 300);
};
</script>
