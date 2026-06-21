
<template>
    <section class="about-title relative h-screen w-full overflow-hidden bg-black text-white">
        <div class="text-center">
            <h2 class="text-4xl font-bold text-white">
                About Us
            </h2>

            <p class="text-gray-400 mt-6">
                We create powerful digital experiences.
            </p>
        </div>
        <div class="absolute left-[70%] top-1/2 -translate-y-1/2 w-[280px]">
            <video autoplay muted loop playsinline class="w-full h-[400px] object-cover rounded-lg">
                <source src="../assets/marked.mp4" type="video/mp4" />
            </video>
        </div>

        <!-- Logo -->
        <div class="fixed top-10 right-12 z-50">
            <h1 class="text-4xl font-black">MC</h1>
        </div>

        <!-- Right Menu -->
        <div class="fixed right-10 top-[160px] z-50 flex flex-col gap-4">

            <router-link to="/" class="group flex items-center justify-center w-10 h-24">
                <span
                    class="[writing-mode:vertical-rl] text-xs tracking-[3px] font-bold uppercase transition-all duration-300 group-hover:text-white group-hover:scale-110">
                    WORKS
                </span>
            </router-link>

            <router-link to="/about" class="group flex items-center justify-center w-10 h-24">
                <span
                    class="[writing-mode:vertical-rl] text-xs tracking-[3px] font-bold uppercase text-zinc-500 transition-all duration-300 group-hover:text-white group-hover:scale-110">
                    ABOUT
                </span>
            </router-link>

            <router-link to="/contact" class="group flex items-center justify-center w-10 h-24">
                <span
                    class="[writing-mode:vertical-rl] text-xs tracking-[3px] font-bold uppercase text-zinc-500 transition-all duration-300 group-hover:text-white group-hover:scale-110">
                    CONTACT
                </span>
            </router-link>

        </div>

        <!-- Scroll Indicator -->
        <div class="fixed bottom-10 right-12 flex flex-col items-center gap-4">
            <span class="text-[10px] tracking-[4px] uppercase rotate-90 origin-center">
                Scroll
            </span>

            <div class="w-[1px] h-24 bg-white"></div>
        </div>

        <!-- Left Panel -->
        <div class="absolute left-[6%] top-1/2 -translate-y-1/2 w-[230px] border-l border-zinc-700 pl-6">
            <div class="mb-12">
                <p class="text-zinc-500 text-xs mb-3 uppercase">
                    Agency
                </p>

                <h3 class="text-3xl font-bold leading-tight">
                    Marked <br />
                    Communication
                </h3>
            </div>

            <div class="mb-12">
                <p class="text-zinc-500 text-xs mb-3 uppercase">
                    Services
                </p>

                <p class="text-lg leading-9">
                    Branding <br />
                    Web Design <br />
                    Marketing
                </p>
            </div>

            <div class="mb-12">
                <p class="text-zinc-500 text-xs mb-3 uppercase">
                    Est.
                </p>

                <p class="text-lg">
                    2024
                </p>
            </div>

            <p class="text-sm text-zinc-500 leading-7">
                We create meaningful digital experiences through
                strategy, design and technology.
            </p>
        </div>

        <!-- Main Content -->
        <div class="absolute left-[20%] top-1/2 -translate-y-1/2">

            <!-- Description -->
            <div class="ml-[26%] mt-8 max-w-[700px]">
                <h2 class="text-[4vw] font-black leading-none mb-6">
                    Creative Digital Studio
                </h2>

                <p class="text-zinc-400 text-[1.3vw] leading-relaxed">
                    We build brands, websites and digital experiences
                    that connect businesses with people.
                </p>

                <p class="text-zinc-400 text-[1.3vw] leading-relaxed mt-3">
                    Through creativity, innovation and strategy we
                    help brands leave a lasting impression.
                </p>
            </div>
        </div>

    </section>
</template>

<script setup>
import { onMounted } from "vue";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

onMounted(() => {
    // Reset body overflow settings when loading
    document.body.style.overflow = "auto";

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
