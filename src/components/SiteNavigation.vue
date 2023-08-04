<template>
  <header class="sticky top-0 bg-weather-p shadow-lg">
    <nav
      class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6"
    >
      <RouterLink :to="{ name: 'home' }">
        <div class="flex items-center gap-3">
          <i class="fa-solid fa-sun text-2xl"></i>
          <p class="text-2xl">The local weather</p>
        </div>
      </RouterLink>

      <div class="flex gap-3 flex-1 justify-end">
        <i
          class="fa-solid fa-circle-info text-xl hover:text-weather-s duration-150 cursor-pointer"
          @click="toggleModal"
        ></i>
        <i
          class="fa-solid fa-plus text-xl hover:text-weather-s duration-150 cursor-pointer"
          @click="addCity"
          v-if="route.query.preview"
        ></i>
        <i
          class="fa-solid fa-trash text-xl hover:text-weather-s duration-150 cursor-pointer"
          @click="removeCity"
          v-if="!route.query.preview"
        ></i>
      </div>
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

const savedCities = ref([]);
const route = useRoute();
const router = useRouter();
const addCity = () => {
  if (localStorage.getItem("savedCities")) {
    savedCities.value = JSON.parse(localStorage.getItem("savedCities"));
  }
  const locationObj = {
    id: uid(),
    state: route.params.state,
    city: route.params.city,
    coords: {
      lat: route.query.lat,
      lng: route.query.lng,
    },
  };

  savedCities.value.push(locationObj);
  localStorage.setItem("savedCities", JSON.stringify(savedCities.value));

  let query = Object.assign({}, route.query);
  delete query.preview;
  router.replace({ query });
};

const modalActive = ref(null);
const toggleModal = () => {
  modalActive.value = !modalActive.value;
};
</script>
