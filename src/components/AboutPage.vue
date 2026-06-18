<template>
    <section class="min-h-screen bg-black flex items-center justify-center">
        <div class="text-center">
            <h2 class="about-title text-6xl font-bold text-white">
                About Us
            </h2>

            <p class="text-gray-400 mt-6 max-w-xl">
                We create powerful digital experiences.
            </p>
        </div>
    </section>
</template>
<script setup>
import { onMounted } from "vue";
import gsap from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

onMounted(() => {
    gsap.from(".about-title", {
        y: 100,
        opacity: 0,
        duration: 1,
        scrollTrigger: {
            trigger: ".about-title",
            end: "top 80%",
        },
    });
});
</script>