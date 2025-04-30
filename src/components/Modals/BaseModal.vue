<script setup lang="ts">
import {defineEmits, defineProps, onBeforeUnmount, onMounted, watch} from "vue";

const props = defineProps<{
  modelValue: boolean;
}>();

const emit = defineEmits<{
  (e: "update:modelValue", value: boolean): void;
}>();

const close = () => {
  emit("update:modelValue", false);
};

const onBackdropClick = () => {
  close();
};

// Scroll Lock Logic
const lockScroll = () => {
  document.body.style.overflow = "hidden";
};

const unlockScroll = () => {
  document.body.style.overflow = "";
};

watch(
    () => props.modelValue,
    (newVal) => {
      if (newVal) {
        lockScroll();
      } else {
        unlockScroll();
      }
    }
);

onMounted(() => {
  if (props.modelValue) lockScroll();
});

onBeforeUnmount(() => {
  unlockScroll(); // на всякий случай
});
</script>

<template>
  <transition name="slide-down">
    <div
        v-if="modelValue"
        class="fixed inset-0 z-[100] flex items-center justify-center px-4"
    >
      <div class="fixed inset-0 bg-black/60" @click="onBackdropClick"></div>

      <div
          class="relative bg-white rounded-xl shadow-xl max-w-lg w-full max-h-[90vh] z-50 overflow-hidden"
      >
        <button
            @click="close"
            class="absolute top-2 right-3 text-3xl text-black cursor-pointer"
        >
          ×
        </button>

        <!-- Добавлено: обёртка со скроллом -->
        <div class="overflow-y-auto max-h-[80vh] p-8">
          <slot/>
        </div>
      </div>
    </div>
  </transition>
</template>

<style scoped>
.slide-down-enter-active,
.slide-down-leave-active {
  transition: all 0.4s ease;
}

.slide-down-enter-from,
.slide-down-leave-to {
  opacity: 0;
  transform: translateY(-30px);
}

.slide-down-enter-to,
.slide-down-leave-from {
  opacity: 1;
  transform: translateY(0);
}

</style>

