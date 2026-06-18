<template>
    <div class="logo-loader fixed inset-0 bg-black z-[9999]
    flex flex-col items-center justify-center">

        <!-- Logo -->
        <img src="../assets/logo.jpg" class="logo-img w-40 mb-8" />

        <!-- Text -->
        <h1 class="welcome text-white text-6xl md:text-7xl
font-black tracking-widest text-center overflow-hidden">

            <span v-for="(letter, index) in letters" :key="index" class="letter inline-block">
                {{ letter }}
            </span>

        </h1>

    </div>
</template>


<script setup>

import { onMounted, ref } from "vue";
import gsap from "gsap";


const text = "WELCOME TO MARKED";
const letters = ref(text.split(""));



onMounted(() => {


    const tl = gsap.timeline();


    tl.from(".logo-img", {

        scale: 0,
        opacity: 0,
        filter: "blur(20px)",
        duration: 1.2,
        ease: "power4.out"

    })


        .from(".letter", {

            y: 100,
            opacity: 0,
            rotateX: 90,
            duration: 0.8,
            stagger: 0.05,
            ease: "power4.out"

        })


        .to(".letter", {

            color: "#a3ff12",
            duration: 0.5,
            stagger: 0.03

        })


        .to(".logo-loader", {

            y: "-100%",
            duration: 1.2,
            ease: "power4.inOut",
            delay: .5

        })


})

</script>