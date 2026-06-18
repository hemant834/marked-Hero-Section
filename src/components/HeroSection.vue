<template>
    <section class="relative h-screen overflow-hidden flex items-center justify-center bg-black">
        <div class="glow absolute w-[500px] h-[500px]
  bg-lime-500/20 blur-[150px]
  rounded-full pointer-events-none"></div>
        <!-- 🎥 Background Video -->
        <video autoplay muted loop playsinline class="absolute inset-0 w-full h-full object-cover opacity-40">
            <source src="../assets/hero.mp4" type="video/mp4" />
        </video>

        <!-- 🌑 Dark Overlay -->
        <div class="absolute inset-0 bg-black/50"></div>

        <!-- Content -->
        <div class="relative z-10 text-center">
            <p class="hero-sub text-lime-400 tracking-[10px] uppercase mb-6">
                Digital Marketing Agency
            </p>

            <h1 class="hero-title text-white font-black uppercase leading-none">
                <span class="block text-6xl md:text-8xl">Grow</span>
                <span class="block text-6xl md:text-8xl">Your Brand</span>
            </h1>

            <p class="mt-6 text-gray-300 max-w-xl mx-auto md:text-3xl">
                We're the coolkids of marketing!
            </p>

            <button class="group relative overflow-hidden mt-10
 px-10 py-4 rounded-full border border-lime-400">
                <span class="relative z-10 text-lime-400 group-hover:text-black">
                    Get Started
                </span>

                <span class="absolute inset-0 bg-lime-400
   translate-y-full
   group-hover:translate-y-0
   transition duration-500"></span>
            </button>
        </div>

        <!-- ⬇ Scroll Indicator -->
        <div class="absolute bottom-10 left-1/2
 -translate-x-1/2 flex flex-col items-center">
            <p class="text-white mb-4 tracking-widest">
                Scroll Down
            </p>

            <div class="w-12 h-12 rounded-full
   border border-white
   flex items-center justify-center
   animate-bounce">
                ↓
            </div>
        </div>
    </section>
</template>

<script setup>
import { onMounted } from "vue";
import gsap from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

onMounted(() => {
    // Title animation
    gsap.from(".hero-title span", {
        y: 120,
        opacity: 0,
        duration: 1.2,
        stagger: 0.2,
        ease: "power4.out",
    });

    // Subtitle animation
    gsap.from(".hero-sub", {
        opacity: 0,
        y: 30,
        duration: 1,
        delay: 0.3,
    });

    gsap.to(".hero-title", {
        y: -150,
        opacity: 0,
        scrollTrigger: {
            trigger: "section",
            start: "bottom 80%",
            end: "bottom 20%",
            scrub: true,
        },
    });

    gsap.to(".hero-sub", {
        y: -100,
        opacity: 0,
        scrollTrigger: {
            trigger: "section",
            start: "bottom 80%",
            end: "bottom 20%",
            scrub: true,
        },
    });

    // Mouse parallax effect
    window.addEventListener("mousemove", (e) => {
        const x = (e.clientX / window.innerWidth - 0.5) * 40;
        const y = (e.clientY / window.innerHeight - 0.5) * 40;

        gsap.to(".hero-title", {
            x,
            y,
            duration: 1,
        });
    });

    window.addEventListener("mousemove", (e) => {
        gsap.to(".glow", {
            x: e.clientX - 250,
            y: e.clientY - 250,
            duration: 1,
        });
    });
});
</script>