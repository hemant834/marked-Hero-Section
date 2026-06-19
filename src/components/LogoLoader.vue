<!-- <template>
    <div class="logo-loader fixed inset-0 bg-black z-[9999]
    flex flex-col items-center justify-center">

=        <div class="preloader">
            <img src="/card.svg" alt="Loading" class="loader-icon" />

        </div>

    </div>
</template>

<style scoped>
.preloader {
    position: fixed;
    inset: 0;
    background: #000;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 9999;
    transition: opacity 0.8s ease;
}

.preloader.is-hidden {
    opacity: 0;
    pointer-events: none;
}

.loader-icon {
    width: 60px;
    animation: pulse 2s infinite ease-in-out;
}
</style>
<script setup>
//
</script> -->
<template>
    <div class="preloader">
        <div class="card-loader">
            <img src="/card.svg" class="card c1" />
            <img src="/card.svg" class="card c2" />
            <img src="/card.svg" class="card c3" />
            <img src="/card.svg" class="card c4" />
            <img src="/card.svg" class="card c5" />
        </div>
    </div>
</template>
<script setup>
import { onMounted } from 'vue';
import gsap from 'gsap';

onMounted(() => {
    // gsap.to(".c1", {
    //     y: -10,
    //     repeat: -1,
    //     yoyo: true,
    //     duration: 1.4,
    // });

    // gsap.to(".c2", {
    //     y: -15,
    //     repeat: -1,
    //     yoyo: true,
    //     duration: 1.4,
    //     delay: 0.1,
    // });

    // gsap.to(".c3", {
    //     y: -20,
    //     repeat: -1,
    //     yoyo: true,
    //     duration: 1.4,
    //     delay: 0.2,
    // });

    // gsap.to(".c4", {
    //     y: -15,
    //     repeat: -1,
    //     yoyo: true,
    //     duration: 1.4,
    //     delay: 0.3,
    // });

    // gsap.to(".c5", {
    //     y: -10,
    //     repeat: -1,
    //     yoyo: true,
    //     duration: 1.4,
    //     delay: 0.4,
    // });
});
</script>
<style>
.preloader {
    position: fixed;
    inset: 0;
    background: #000;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 9999;
}

.card-loader {
    position: relative;
    width: 300px;
    height: 180px;
}

.card {
    position: absolute;
    width: 90px;
    left: 50%;
    top: 50%;
    transform-origin: bottom center;
    animation: floatCard 2.5s ease-in-out infinite;
}

.c1 {
    transform: translate(-50%, -50%) rotate(-25deg);
    z-index: 1;
}

.c2 {
    transform: translate(-50%, -50%) rotate(-12deg);
    z-index: 2;
}

.c3 {
    transform: translate(-50%, -50%) rotate(0deg);
    z-index: 3;
}

.c4 {
    transform: translate(-50%, -50%) rotate(12deg);
    z-index: 2;
}

.c5 {
    transform: translate(-50%, -50%) rotate(25deg);
    z-index: 1;
}

.c1 {
    animation-delay: 0s;
}

.c2 {
    animation-delay: .15s;
}

.c3 {
    animation-delay: .3s;
}

.c4 {
    animation-delay: .45s;
}

.c5 {
    animation-delay: .6s;
}

@keyframes floatCard {

    0%,
    100% {
        margin-top: 0;
    }

    50% {
        margin-top: -12px;
    }
}
</style>