<template>
  <HeroSection />
  <!-- <AboutPage /> -->
  <LogoLoader v-if="loading" />
</template>

<script setup>
import { ref, onMounted } from "vue";
import HeroSection from "../components/HeroSection.vue";
// import AboutPage from "../components/AboutPage.vue";
import LogoLoader from "../components/LogoLoader.vue";
import gsap from 'gsap';

const loading = ref(true);
setTimeout(() => {
  gsap.to(".preloader", {
    opacity: 0,
    duration: 1,
    onComplete: () => {
      loading.value = false;
    }
  });
}, 3000);
</script>