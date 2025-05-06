<script setup lang="ts">
import {defineEmits, defineProps, onBeforeUnmount, onMounted, watch} from "vue";

const props = defineProps<{
  modelValue: boolean;
}>();

const emit = defineEmits<{
  (e: "update:modelValue", value: boolean): void;
}>();

const close = () => emit("update:modelValue", false);
const onBackdropClick = () => close();

// Scroll Lock
const lockScroll = () => {
  document.body.style.overflow = "hidden";
};
const unlockScroll = () => {
  document.body.style.overflow = "";
};

watch(() => props.modelValue, (val) => {
  val ? lockScroll() : unlockScroll();
});
onMounted(() => {
  if (props.modelValue) lockScroll();
});
onBeforeUnmount(() => unlockScroll());
</script>

<template>
  <div v-if="modelValue" class="fixed inset-0 z-[100] flex items-center justify-center px-2">
    <!-- Фон с fade-появлением -->
    <transition name="fade-backdrop">
      <div
          v-show="modelValue"
          class="fixed inset-0 bg-black/60"
          @click="onBackdropClick"
      ></div>
    </transition>

    <!-- Модалка с плавным появлением сверху -->
    <transition name="slide-down" appear>
      <div
          v-if="modelValue"
          class="relative bg-white rounded-xl shadow-xl max-w-lg w-full max-h-[90vh] z-50 overflow-hidden"
      >
        <button
            @click="close"
            class="absolute top-2 right-3 text-3xl text-black cursor-pointer"
        >
          ×
        </button>

        <div class="overflow-y-auto  p-8 scrollbar-hide">

          <slot/>
        </div>
      </div>
    </transition>
  </div>
</template>

<style scoped>
/* Анимация заднего фона */
.fade-backdrop-enter-active,
.fade-backdrop-leave-active {
  transition: opacity 0.25s ease;
}

.fade-backdrop-enter-from,
.fade-backdrop-leave-to {
  opacity: 0;
}

.fade-backdrop-enter-to,
.fade-backdrop-leave-from {
  opacity: 1;
}

/* Анимация модального окна */
.slide-down-enter-active {
  transition: all 0.4s ease;
}

.slide-down-enter-from {
  transform: translateY(-40px);
  opacity: 0;
}

.slide-down-enter-to {
  transform: translateY(0);
  opacity: 1;
}
</style>
