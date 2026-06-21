<template>
  <div class="relative w-full min-h-screen bg-black overflow-x-hidden">
    <!-- AboutPage rendered in the background -->
    <AboutPage />
    <!-- HeroSection overlays AboutPage and closes on transition complete -->
    <HeroSection v-if="showHero" @close="showHero = false" />
    <LogoLoader v-if="loading" />
  </div>
</template>

<script setup>
import { ref } from "vue";
import HeroSection from "../components/HeroSection.vue";
import AboutPage from "../components/AboutPage.vue";
import LogoLoader from "../components/LogoLoader.vue";
import gsap from 'gsap';

const loading = ref(true);
const showHero = ref(true);

setTimeout(() => {
  gsap.to(".preloader", {
    opacity: 0,
    duration: 1,
    onComplete: () => {
      loading.value = false;
    }
  });
}, 1000);
</script>