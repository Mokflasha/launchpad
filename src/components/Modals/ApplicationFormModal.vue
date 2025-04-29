<script setup lang="ts">
import {ref} from "vue";
import BaseModal from "@/components/Modals/BaseModal.vue";
import Input from "@/components/Base/Input.vue";

const props = defineProps<{
  modelValue: boolean;
  openPrivacyPolicy: () => void;
}>();

const name = ref("");
const email = ref("");
const phone = ref("");
const team = ref("");

const fields = [
  {id: "name", label: "ФИО", type: "text", model: name},
  {id: "email", label: "Email", type: "email", model: email},
  {id: "phone", label: "Телефон", type: "tel", model: phone},
  {id: "team", label: "Организация / Команда", type: "text", model: team},
];
</script>

<template>
  <BaseModal v-model="props.modelValue">
    <div class="relative w-full h-full pt-6 pb-12 px-16">
      <h2 class="text-md font-medium my-8 text-center">
        Отправь заявку на чемпионат прямо сейчас!
      </h2>

      <form class="flex flex-col gap-4">
        <div v-for="(field, index) in fields" :key="index" class="flex flex-col gap-1">
          <Input
              v-model="field.model.value"
              :id="field.id"
              :type="field.type"
              :label="field.label"
              required
          />
        </div>

        <div class="flex flex-col gap-1">
          <label for="weight" class="text-sm font-normal">
            Вес <span>*</span>
          </label>
          <select
              id="weight"
              class="border p-2 rounded-md focus:outline-[#5029de] bg-white h-10 text-zinc-900"
          >
            <option value="40">40</option>
            <option value="41">41</option>
            <option value="42">42</option>
          </select>
        </div>

        <button
            type="submit"
            class="mt-2 bg-[#5029de] text-white py-2 rounded-md hover:bg-[#4124ab] transition shadow-[0_5px_5px_#00000050]"
        >
          Зарегистрироваться
        </button>

        <p class="text-[.65em] text-center text-zinc-600">
          Нажимая кнопку, вы даете согласие на
          <a @click="props.openPrivacyPolicy" class="underline cursor-pointer">
            обработку персональных данных
          </a>
        </p>
      </form>
    </div>
  </BaseModal>
</template>
