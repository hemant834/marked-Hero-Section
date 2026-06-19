<template>
    <!-- <section class="hero-section relative h-screen overflow-hidden flex items-center justify-center bg-black"> -->
    <section ref="heroPanel"
        class="hero-section relative h-screen overflow-hidden flex items-center justify-center bg-black">
        <div class="glow absolute w-[500px] h-[500px]
  bg-lime-500/20 blur-[150px]
  rounded-full pointer-events-none"></div>
        <!-- 🎥 Background Video -->
        <video autoplay muted loop playsinline class="absolute inset-0 w-full h-full object-cover opacity-40">
            <source src="../assets/title_01.png" type="herobackgroud" />
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

<!-- <script setup>
import { ref, onMounted } from "vue";
import gsap from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";
import { useRouter } from "vue-router";

gsap.registerPlugin(ScrollTrigger);
const router = useRouter();

const heroPanel = ref(null);

const TOP_XS = [
    0, 12.5, 25, 37.5,
    50, 62.5, 75, 87.5, 100
];

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

    // CHANGED: Direct About Page Open
    let isNavigating = false;

    window.addEventListener("wheel", (e) => {
        if (e.deltaY > 0 && !isNavigating) {
            isNavigating = true;
            router.push("/about");
        }
    });

});

</script> -->

<script setup>
import { ref, onMounted } from "vue";
import gsap from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";
import { useRouter } from "vue-router";

gsap.registerPlugin(ScrollTrigger);

const router = useRouter();
const heroPanel = ref(null);

const TOP_XS = [
    0,
    12.5,
    25,
    37.5,
    50,
    62.5,
    75,
    87.5,
    100,
];

let isAnimating = false;
let wheelCount = 0;

function runWave() {

    if (isAnimating) return;

    isAnimating = true;

    // Scroll Lock
    document.body.style.overflow = "hidden";

    const pts = {
        y0: 0,
        y1: 0,
        y2: 0,
        y3: 0,
        y4: 0,
        y5: 0,
        y6: 0,
        y7: 0,
        y8: 0
    };

    const updateClip = () => {

        const top = TOP_XS.map(
            (x, i) => `${x}% ${pts["y" + i]}%`
        ).join(",");

        heroPanel.value.style.clipPath =
            `polygon(${top},100% 100%,0% 100%)`;

    };

    const tl = gsap.timeline({

        onComplete: () => {

            router.push("/about");

        }

    });

    // Hero Content Hide
    tl.to(
        ".hero-title, .hero-sub, button",
        {
            opacity: 0,
            duration: 0.4
        },
        0
    );

    // Right → Left Wave
    TOP_XS.forEach((_, i) => {

        const key = "y" + i;

        const rightIndex = 8 - i;

        const startAt = 0.1 + rightIndex * 0.18;

        tl.to(
            pts,
            {
                [key]: 200,
                duration: 2,
                ease: "power4.in",
                onUpdate: updateClip
            },
            startAt
        );

    });

}

onMounted(() => {
    // Initial shape
    heroPanel.value.style.clipPath = `
polygon(
0% 0%,
12.5% 0%,
25% 0%,
37.5% 0%,
50% 0%,
62.5% 0%,
75% 0%,
87.5% 0%,
100% 0%,
100% 100%,
0% 100%
)
`;

    // Hero Intro
    gsap.from(".hero-title span", {
        y: 120,
        opacity: 0,
        duration: 1.2,
        stagger: 0.2,
        ease: "power4.out",
    });

    gsap.from(".hero-sub", {
        y: 30,
        opacity: 0,
        duration: 1,
        delay: 0.3,
    });

    // Mouse Parallax
    window.addEventListener("mousemove", (e) => {
        const x =
            (e.clientX / window.innerWidth - 0.5) * 40;

        const y =
            (e.clientY / window.innerHeight - 0.5) * 40;

        gsap.to(".hero-title", {
            x,
            y,
            duration: 1,
        });

        gsap.to(".glow", {
            x: e.clientX - 250,
            y: e.clientY - 250,
            duration: 1,
        });
    });

    let isAnimating = false;

    window.addEventListener(
        "wheel",
        (e) => {

            if (isAnimating) return;

            wheelCount += e.deltaY;

            if (wheelCount > 0) {

                wheelCount = 0;

                runWave();
            }

        },
        { passive: true }
    );
});
</script>
<style scoped>
.hero-section {
    width: 100%;
    height: 100vh;
    position: relative;
    overflow: hidden;
}
</style>