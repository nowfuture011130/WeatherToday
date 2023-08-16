<template>
  <div>
    <!-- Suspense里的内容都带有async函数，因此它需要fallback指定当数据未显示前应该显示什么 -->
    <Suspense>
      <AsyncCityView />
      <template #fallback>
        <CityViewSkeleton />
      </template>
    </Suspense>
  </div>
</template>

<script setup>
import AsyncCityView from "../components/AsyncCityView.vue";
import CityViewSkeleton from "../components/CityViewSkeleton.vue";
</script>
