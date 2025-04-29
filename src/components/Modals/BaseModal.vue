<script setup lang="ts">
import {defineEmits, defineProps} from "vue";

const props = defineProps<{
  modelValue: boolean;
}>();

const emits = defineEmits<{
  (e: "update:modelValue", value: boolean): void;
}>();

function close() {
  emits("update:modelValue", false);
}

function onBackdropClick() {
  close();
}
</script>

<template>
  <transition name="fade">
    <div v-if="props.modelValue" class="fixed inset-0 z-[100] flex items-center justify-center px-4">
      <div class="fixed inset-0 bg-black/60" @click="onBackdropClick"></div>

      <div
          class="relative bg-white rounded-xl shadow-xl max-w-lg w-full max-h-[90vh] p-8 z-50"
      >
        <button
            @click="close"
            class="absolute top-2 right-3 text-3xl cursor-pointer"
        >
          ×
        </button>

        <!-- Динмический контент -->
        <slot/>
      </div>
    </div>
  </transition>
</template>

<style scoped>

</style>