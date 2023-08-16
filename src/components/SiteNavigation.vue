<template>
  <header class="sticky top-0 bg-weather-p shadow-lg">
    <!-- 导航栏包裹 -->
    <nav
      class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6"
    >
      <!-- 将整个标题添加上回到home的功能 -->
      <RouterLink :to="{ name: 'home' }">
        <!-- 太阳图标和标题的显示 -->
        <div class="flex items-center gap-3">
          <i class="fa-solid fa-sun text-2xl"></i>
          <p class="text-2xl">The local weather</p>
        </div>
      </RouterLink>

      <div class="flex gap-3 flex-1 justify-end">
        <!-- 感叹号的图标 toggleModal实现了点击后弹出关于内容的效果-->
        <i
          class="fa-solid fa-circle-info text-xl hover:text-weather-s duration-150 cursor-pointer"
          @click="toggleModal"
        ></i>
        <!-- 加号图标 -->
        <i
          class="fa-solid fa-plus text-xl hover:text-weather-s duration-150 cursor-pointer"
          @click="addCity"
          v-if="route.query.preview"
        ></i>
        <!-- 删除图标 -->
        <i
          class="fa-solid fa-trash text-xl hover:text-weather-s duration-150 cursor-pointer"
          @click="removeCity"
          v-if="!route.query.preview && route.name !== 'home'"
        ></i>
      </div>
      <!-- :modal-active为传递值 @close-modal为传递方法 -->
      <BaseModal :modal-active="modalActive" @close-modal="toggleModal">
        <div class="text-black">
          <h1 class="text-2xl mb-1">About:</h1>
          <p>xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.</p>
          <p>xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.</p>
          <p>xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.</p>
        </div>
      </BaseModal>
    </nav>
  </header>
</template>

<script setup>
import { ref } from "vue";
import BaseModal from "./BaseModal.vue";
import { uid } from "uid";
import { RouterLink, useRoute, useRouter } from "vue-router";

const savedCities = ref([]); //保存城市
const route = useRoute();
const router = useRouter();
// 向内存添加城市
const addCity = () => {
  // 先从内存中获得存储的城市
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(localStorage.getItem("savedCities"));
  }
  // 创建城市
  const locationObj = {
    id: uid(),
    state: route.params.state,
    city: route.params.city,
    coords: {
      lat: route.query.lat,
      lng: route.query.lng,
    },
  };

  // 然后向内存中添加新城市
  savedCities.value.push(locationObj);
  localStorage.setItem("savedCities", JSON.stringify(savedCities.value));

  // 没完全搞懂
  let query = Object.assign({}, route.query);
  delete query.preview;
  query.id = locationObj.id;
  router.replace({ query });
};
// 从内存中删除城市
const removeCity = () => {
  const cities = JSON.parse(localStorage.getItem("savedCities"));
  const updatedCities = cities.filter((city) => city.id !== route.query.id);
  localStorage.setItem("savedCities", JSON.stringify(updatedCities));
  router.push({ name: "home" });
};

// 定义了一个变量(可被Vue监测到)
const modalActive = ref(null);
// 定义了一个方法(可被Vue监测到)
const toggleModal = () => {
  modalActive.value = !modalActive.value;
};
</script>
