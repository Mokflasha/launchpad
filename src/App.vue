<script setup lang="ts">
import ApplicationFormModal from "@/components/Modals/ApplicationFormModal.vue";
import PrivacyPolicyModal from "@/components/Modals/PrivacyPolicyModal.vue";
import CountdownTimer from "@/components/CountdownTimer.vue";
import Button from "@/components/Base/Button.vue";
import {useCountdown} from "@/composables/useCountdown";
import {useModalController} from "@/composables/useModalController";
import {ref} from "vue";
import FeeModal from "@/components/Modals/FeeModal.vue";
import EntryConditionsModal from "@/components/Modals/EntryConditionsModal.vue";
import PrivacyAgreementModal from "@/components/Modals/PrivacyAgreementModal.vue";
import PolicyModal from "@/components/Modals/PolicyModal.vue";

const TOURNAMENT_DATE = "2025-05-24T00:00:00";

const showFeeModal = ref(false);
const showConditionsModal = ref(false);
const showPrivacyAgreement = ref(false);
const showPolicy = ref(false);

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
  openForm,
  openPrivacyPolicy
} = useModalController();

const isMobileMenuOpen = ref(false);
const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value;
};
</script>

<template>
  <div class="min-h-screen w-full relative overflow-x-hidden" :class="showForm ? 'touch-none' : ''">
    <div class="flex justify-between sm:justify-center p-5 items-center">
      <div class="flex sm:flex-col items-center justify-center">
        <img src="./assets/images/logo.png" alt="logo" class="w-[75px]"/>
        <p class="text-white font-extrabold ml-2 mt-2 w-[200px] sm:w-full">Alliance Cup Championship</p>
      </div>
      <button @click="toggleMobileMenu" class="sm:hidden text-white w-8 h-8" aria-label="Toggle menu">
        <svg v-if="!isMobileMenuOpen" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
             stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"/>
        </svg>
        <svg v-else xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
        </svg>
      </button>
    </div>
    <div
        class="absolute inset-0 -z-10 bg-[url(./assets/images/background.jpg)] bg-no-repeat bg-cover bg-[center_66%]"></div>
    <div class="fixed inset-0 -z-10 bg-black/50 min-h-screen h-full"></div>
    <ApplicationFormModal v-model="showForm" :openPrivacyPolicy="openPrivacyPolicy"/>
    <PrivacyPolicyModal v-model="showPrivacyPolicy"/>
    <transition name="fade">
      <div v-if="isMobileMenuOpen"
           class="sm:hidden flex flex-col items-center text-center text-white p-5 bg-black/50">
        <p class="text-base font-medium">
          Соревнования по единоборствам: грепплинг, панкратион, вольная, греко-римская борьба.
        </p>
        <!--
        здесь тоже я указал каркас для того, чтоб человек,при нажатии на номер
        телефона мог позвонить, если будут вопросы
        тебенужно просто вместо этого номера вставить номер на который будут совершаться звонки
        в таком же формате как я указал тестовый номер
        -->
        <a href="tel:+71234567890" class="text-lg font-semibold mt-4">+7 (123) 456-78-90</a>
        <p class="text-sm text-gray-300">техподдержка</p>
      </div>
    </transition>

    <FeeModal v-model="showFeeModal"/>
    <div class="text-white px-[5vw] sm:pt-14 max-sm:pt-8 flex flex-col items-center gap-4">

      <div class="flex flex-col gap-2">
        <div
            class="flex flex-col justify-evenly items-center border-2 border-[#44008C] bg-black/50 px-[2vw] py-[2vh] h-[250px] sm:h-auto
         shadow-[0_8px_20px_#44008C]">
          <h1 class="sm:mb-0 mb-4 uppercase text-stroke text-center text-2xl text-glow">
            Open Grappling Tournament 2025
          </h1>
          <!--    Где href нужно будет ввести ссылку на расписание оно уже откроется на новой странице      -->
          <a href="#" target="_blank" class="
          sm:hidden text-[clamp(.9rem,1.5vw,1.25rem)] rounded-3xl uppercase w-fit
          px-8 py-3 font-medium transition shadow-md
          bg-gradient-to-r from-[#5029de] to-[#4124ab] text-white
          ">
            РАСПИСАНИЕ ТУРНИРА
          </a>
        </div>

        <div class="text-center font-medium text-[clamp(1rem,2vw,1.5rem)]">
          <h2 class="text-sm mb-4">Махачкала, Дворец спорта "Автодор" <br> 24–25 Мая, 2025</h2>
          <Button @click="openForm" variant="primary"
                  class="sm:hidden text-[clamp(.9rem,1.5vw,1.25rem)] uppercase w-fit">
            зарегистрироваться
          </Button>
        </div>
      </div>
      <EntryConditionsModal v-model="showConditionsModal"/>
      <PrivacyAgreementModal v-model="showPrivacyAgreement"/>
      <PolicyModal v-model="showPolicy"/>

      <!-- Register & Timer -->
      <div class="flex flex-col-reverse sm:flex-col gap-4 w-full items-center">
        <nav
            class="flex flex-col sm:grid gap-2 py-4 uppercase text-center text-[clamp(0.9rem,1.2vw,1.1rem)]  grid-cols-2">
          <h3 class="sm:hidden font-normal text-base">О турнире:</h3>

          <a @click="showFeeModal = true"
             class="border border-[#5029de] bg-black/50 rounded-3xl px-4 py-5 font-normal text-sm">
            Оплата взноса и страховка
          </a>

          <a @click="showConditionsModal = true"
             class="border border-[#5029de] bg-black/50 rounded-3xl px-4 py-5 font-normal text-sm">
            Условия допуска
          </a>

          <!--     сюда закидываешь ту ссылку на регламент  href="#" вместо #, ссылка на регламент    -->
          <a download class="border border-[#5029de] bg-black/50 rounded-3xl px-4 py-5 font-normal text-sm"
             href="#">
            Скачать регламент
          </a>
          <a class="border border-[#5029de] bg-black/50 rounded-3xl px-4 py-5 font-normal text-sm" href="#">
            Список участников
          </a>

        </nav>
        <Button @click="openForm" variant="primary"
                class="max-sm:hidden text-[clamp(.9rem,1.5vw,1.25rem)] uppercase rounded-3xl w-fit">
          зарегистрироваться
        </Button>

        <CountdownTimer
            title="До турнира осталось:"
            :times="[
            { value: days, label: dayLabel },
            { value: hours, label: hourLabel },
            { value: minutes, label: minuteLabel },
            { value: seconds, label: secondLabel }
          ]"
            :showTitle="true"
        />
      </div>

      <!-- Sponsors -->
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
          <a @click="showPrivacyAgreement =true">Согласие на обработку персональных данных</a>
          <a @click="showPolicy = true">Политика конфиденциальности</a>
        </div>
      </div>
    </div>
  </div>
</template>


<style>
.text-glow {
  filter: drop-shadow(4px 5px 40px #B413FF);
}
</style>