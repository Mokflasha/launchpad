<script setup lang="ts">
import {onMounted, ref} from "vue";
import ApplicationFormModal from "@/components/Modals/ApplicationFormModal.vue";
import PrivacyPolicyModal from "@/components/Modals/PrivacyPolicyModal.vue";
import CountdownTimer from "@/components/CountdownTimer.vue";
import Button from "@/components/Base/Button.vue";
import {useCountdown} from "@/composables/useCountdown";
import {useModalController} from "@/composables/useModalController";
import Loading from "@/components/Base/Loading.vue";

const TOURNAMENT_DATE = "2025-05-24T00:00:00";
const isLoading = ref<boolean>(true);

const {
  days,
  hours,
  minutes,
  seconds,
  dayLabel,
  hourLabel,
  minuteLabel,
  secondLabel
} = useCountdown(TOURNAMENT_DATE);

const {
  showForm,
  showPrivacyPolicy,
  firstOpen,
  animateOnce,
  openForm,
  closeForm,
  openPrivacyPolicy,
  closePrivacyPolicy,
} = useModalController();

onMounted(() => {
  setTimeout(() => {
    isLoading.value = false;
  }, 2000);
});
</script>


<template>
  <div
      class="min-h-screen w-full relative overflow-x-hidden"
      :class="showForm ? 'touch-none' : ''"
  >
    <transition name="fade">
      <div v-if="isLoading" class="fixed inset-0 bg-black flex items-center justify-center z-[9999]">
        <Loading/>
      </div>
    </transition>

    <div
        class="absolute inset-0 -z-10 bg-[url(./assets/images/background.jpg)] bg-no-repeat bg-cover bg-[center_66%]"
    ></div>
    <div class="fixed inset-0 -z-10 bg-black/50 min-h-screen h-full"></div>

    <ApplicationFormModal
        v-model="showForm"
        :openPrivacyPolicy="openPrivacyPolicy"
    />
    <PrivacyPolicyModal v-model="showPrivacyPolicy"/>

    <div class="text-white px-[5vw] sm:pt-28 max-sm:pt-8 flex flex-col items-center gap-4 max-sm:gap-16">
      <div class="flex flex-col gap-2">
        <h1
            class="text-[clamp(3rem,5vw,4rem)] border-2 border-[#5029de] bg-black/50 uppercase px-[2vw] py-[2vh] text-stroke text-center max-sm:text-[1rem] max-sm:px-[8vw]"
        >
          alliance championship 2025
        </h1>

        <div class="text-center font-medium text-[clamp(1rem,2vw,1.5rem)] max-sm:text-[.8rem] max-sm:opacity-[.9]">
          <h2>Махачкала, Дворец спорта "Автодор"</h2>
          <h2>24-25 Мая, 2025</h2>
        </div>
      </div>

      <nav
          class="grid grid-cols-2 gap-1 px-[10vw] py-4 bg-black/50 uppercase text-center text-[clamp(0.9rem,1.2vw,1.1rem)] max-sm:hidden"
      >
        <a class="bg-[#5029de] p-2 font-medium" href="#">Расписание турнира</a>
        <a class="bg-[#5029de] p-2 font-medium" href="#">Список участников</a>
        <a class="bg-[#5029de] p-2 font-medium" href="#">Условия допуска</a>
        <a class="bg-[#5029de] p-2 font-medium" href="#">Оплата взноса и страховка</a>
      </nav>

      <div class="flex flex-col gap-4 w-full items-center">
        <Button @click="openForm" variant="primary"
                class="text-[clamp(.9rem,1.5vw,1.25rem)] uppercase max-sm:mt-96 w-fit">
          зарегистрироваться
        </Button>

        <CountdownTimer
            title="До турнира осталось:"
            :times="[
                { value: days, label: dayLabel },
                { value: hours, label: hourLabel },
                { value: minutes, label: minuteLabel },
                { value: seconds, label: secondLabel },
              ]"
            :showTitle="true"
        />
      </div>

      <nav class="sm:hidden flex flex-col gap-2 w-full mt-6">
        <a class="border-[#5029de] border-2 rounded-md p-2 font-medium text-center active:scale-[.97] bg-black/30"
           href="#">
          Расписание турнира
        </a>
        <a class="border-[#5029de] border-2 rounded-md p-2 font-medium text-center active:scale-[.97] bg-black/30"
           href="#">
          Список участников
        </a>
        <a class="border-[#5029de] border-2 rounded-md p-2 font-medium text-center active:scale-[.97] bg-black/30"
           href="#">
          Условия допуска
        </a>
        <a class="border-[#5029de] border-2 rounded-md p-2 font-medium text-center active:scale-[.97] bg-black/30"
           href="#">
          Оплата взноса
        </a>
      </nav>

      <div class="flex flex-col gap-6 w-full items-center mt-10 sm:absolute sh:relative bottom-0 mb-4">
        <div class="flex flex-col gap-2 items-center">
          <p class="sm:hidden font-semibold">Наши спонсоры:</p>
          <div class="flex flex-row gap-8 items-center">
            <a href="#"><img class="h-[clamp(4rem,10vw,4rem)]" src="./assets/images/mircato.png" alt=""/></a>
            <a href="#"><img class="h-[clamp(6rem,15vw,8rem)]" src="./assets/images/dihard.png" alt=""/></a>
            <a href="#"><img class="h-[clamp(4rem,10vw,4rem)]" src="./assets/images/architeck.png" alt=""/></a>
          </div>
        </div>

        <div class="flex flex-col gap-2 text-[.7rem] font-medium underline text-center sm:hidden">
          <a href="#">Согласие на обработку персональных данных</a>
          <a href="#">Политика конфиденциальности</a>
        </div>
      </div>
    </div>
  </div>
</template>
