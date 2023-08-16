<template>
  <!-- Teleport组件允许你在生成真实网页时将该组件显示在某一组件内并且不影响任何代码实现 -->
  <Teleport to="body">
    <!-- Transition目的是给div提供动画 -->
    <Transition name="modal-outer">
      <!-- 使整个屏幕变暗 -->
      <div
        v-show="modalActive"
        class="absolute w-full bg-black bg-opacity-30 h-screen top-0 left-0 flex justify-center px-8"
      >
        <Transition name="modal-inner">
          <!-- 装载about内容的白盒子 -->
          <div
            v-if="modalActive"
            class="p-4 bg-white self-start mt-32 max-w-screen-md"
          >
            <!-- slot是所有外部内容传进来时被替换为内容 -->
            <slot />
            <!-- @click="$emit('close-modal')这句话实际上是在调用vm上面的$emit方法  -->
            <button
              class="text-white mt-8 bg-weather-p py-2 px-6"
              @click="$emit('close-modal')"
            >
              Close
            </button>
          </div>
        </Transition>
      </div>
    </Transition>
  </Teleport>
</template>

<script setup>
// 使用defineEmits接受了来自父组件的close-modal函数
defineEmits(["close-modal"]);
// 使用defineProps接受了来自父组件的modalActive变量，且将其初始内容改为false
defineProps({
  modalActive: {
    type: Boolean,
    default: false,
  },
});
</script>

<!-- 给整个显示提供动画，所以增加scoped关键词 -->
<style scoped>
.modal-outer-enter-active,
.modal-outer-leave-active {
  transition: opacity 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
}

.modal-outer-enter-from,
.modal-outer-leave-to {
  opacity: 0;
}

.modal-inner-enter-active {
  transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02) 0.15s;
}
.modal-inner-leave-active {
  transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
}

.modal-inner-enter-from {
  opacity: 0;
  transform: scale(0.8);
}
.modal-inner-leave-to {
  transform: scale(0.8);
}
</style>
