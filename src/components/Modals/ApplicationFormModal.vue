<script setup lang="ts">
import {computed, ref} from "vue";
import BaseModal from "@/components/Modals/BaseModal.vue";
import Input from "@/components/Base/Input.vue";
import {useFormValidation} from "@/composables/useFormValidation";
import {citiesByDistrict, districts} from "@/constants/districts";

const props = defineProps<{
  modelValue: boolean;
  openPrivacyPolicy: () => void;
}>();

const emit = defineEmits<{
  (e: "update:modelValue", value: boolean): void;
}>();

const lastName = ref("");
const firstName = ref("");
const middleName = ref("");
const birthYear = ref("");
const weight = ref("");
const club = ref("");
const district = ref("");
const city = ref("");
const coachFullName = ref("");
const phone = ref("");

// Генерация списков
const birthYears = Array.from({length: 2021 - 1961 + 1}, (_, i) => (1961 + i).toString());
const weights = Array.from({length: 130}, (_, i) => (i + 1).toString());
const cities = computed(() => citiesByDistrict[district.value] || []);

// Валидация
const {errors, validateField, validateAll} = useFormValidation();

const onSubmit = (e: Event) => {
  e.preventDefault();
  const valid = validateAll({
    lastName: lastName.value,
    firstName: firstName.value,
    birthYear: birthYear.value,
    weight: weight.value,
    club: club.value,
    district: district.value,
    coachFullName: coachFullName.value,
    phone: phone.value
  });

  if (valid) {
    console.log("✅ Форма валидна");
    // здесь можно отправить данные
  }
};
</script>

<template>
  <BaseModal
      :modelValue="props.modelValue"
      @update:modelValue="val => emit('update:modelValue', val)"
  >
    <div class="w-full max-h-[80vh] overflow-y-auto">
      <h2 class="text-md font-medium my-4 text-center">
        Отправь заявку на чемпионат
      </h2>

      <form @submit="onSubmit" class="flex flex-col gap-4">
        <Input v-model="lastName" id="lastName" label="Фамилия" required @blur="validateField('lastName', lastName)"/>
        <p v-if="errors.lastName" class="text-red-500 text-sm">{{ errors.lastName }}</p>

        <Input v-model="firstName" id="firstName" label="Имя" required @blur="validateField('firstName', firstName)"/>
        <p v-if="errors.firstName" class="text-red-500 text-sm">{{ errors.firstName }}</p>

        <Input v-model="middleName" id="middleName" label="Отчество"/>

        <label class="text-sm font-normal">Год рождения *</label>
        <select v-model="birthYear" class="border p-2 rounded-md text-zinc-900">
          <option disabled value="">Выберите год</option>
          <option v-for="year in birthYears" :key="year" :value="year">{{ year }}</option>
        </select>
        <p v-if="errors.birthYear" class="text-red-500 text-sm">{{ errors.birthYear }}</p>

        <label class="text-sm font-normal">Вес, кг *</label>
        <select v-model="weight" class="border p-2 rounded-md text-zinc-900">
          <option disabled value="">Выберите вес</option>
          <option v-for="w in weights" :key="w" :value="w">{{ w }}</option>
        </select>
        <p v-if="errors.weight" class="text-red-500 text-sm">{{ errors.weight }}</p>

        <Input v-model="club" id="club" label="Клуб" required @blur="validateField('club', club)"/>
        <p v-if="errors.club" class="text-red-500 text-sm">{{ errors.club }}</p>

        <label class="text-sm font-normal">Округ *</label>
        <select v-model="district" class="border p-2 rounded-md text-zinc-900">
          <option disabled value="">Выберите округ</option>
          <option v-for="d in districts" :key="d" :value="d">{{ d }}</option>
        </select>
        <p v-if="errors.district" class="text-red-500 text-sm">{{ errors.district }}</p>

        <label v-if="cities.length" class="text-sm font-normal">Город *</label>
        <select v-if="cities.length" v-model="city" class="border p-2 rounded-md text-zinc-900">
          <option disabled value="">Выберите город</option>
          <option v-for="c in cities" :key="c" :value="c">{{ c }}</option>
        </select>

        <Input v-model="coachFullName" id="coachFullName" label="ФИО тренера" required
               @blur="validateField('coachFullName', coachFullName)"/>
        <p v-if="errors.coachFullName" class="text-red-500 text-sm">{{ errors.coachFullName }}</p>

        <Input v-model="phone" id="phone" label="Номер телефона" type="tel" required
               @blur="validateField('phone', phone)"/>
        <p v-if="errors.phone" class="text-red-500 text-sm">{{ errors.phone }}</p>

        <button type="submit" class="bg-[#5029de] text-white py-2 rounded-md hover:bg-[#4124ab] transition shadow">
          Отправить
        </button>

        <p class="text-[.65em] text-center text-zinc-600 mt-1">
          Нажимая кнопку, вы даёте согласие на
          <a @click="props.openPrivacyPolicy" class="underline cursor-pointer">обработку персональных данных</a>
        </p>
      </form>
    </div>
  </BaseModal>
</template>
