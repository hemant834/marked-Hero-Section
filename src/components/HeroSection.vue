<template>
    <div class="hero-wrapper">
        <section class="hero-section" style="clip-path:url(#hero-clip);">
            <!-- SVG Filter for Wave Distortion -->
            <svg class="hidden-svg">
                <defs>
                    <filter id="wave-filter" x="-20%" y="-20%" width="140%" height="140%">
                        <!-- Base turbulence for the wave noise -->
                        <feTurbulence type="fractalNoise" baseFrequency="0.0046 0.0020" numOctaves="5" result="noise" />
                        <!-- Displacement map to warp the text layout -->
                        <feDisplacementMap in="SourceGraphic" in2="noise" scale="0" xChannelSelector="R"
                            yChannelSelector="G" ref="displacementEl" />
                    </filter>
                </defs>
            </svg>
            <svg class="hidden-svg">
                <defs>
                    <clipPath id="hero-clip" clipPathUnits="objectBoundingBox">
                        <polygon ref="polygonEl" points="0,0 1,0 1,1 0,1" />
                    </clipPath>
                </defs>
            </svg>

            <!-- Header -->
            <header class="header" ref="headerEl">
                <div class="logo">
                    <img src="/logo.svg" alt="Logo">
                </div>

                <button class="menu-btn">
                    <span></span>
                    <span></span>
                </button>
            </header>

            <!-- Hero Text Layout (styled exactly like 2nd image) -->
            <div class="hero-container" ref="containerEl">
                <div class="hero-row-top">
                    <span class="text-make">Make</span>

                    <div class="logo-tagline-container">
                        <img src="/logo.svg" class="hero-logo" alt="Logo" />
                        <div class="tagline-text">
                            <span>CRAFTING DIGITAL EXPERIENCES</span>
                            <span>THROUGH <strong>DESIGN</strong> AND <strong>TECHNOLOGY.</strong></span>
                        </div>
                    </div>
                </div>

                <div class="hero-row-bottom">
                    <span class="text-awesome">AWESOME</span>
                </div>
            </div>

            <!-- Scroll Indicator -->
            <div class="overlay" ref="overlayEl">
                <div class="scroll-indicator">
                    <span>SCROLL</span>
                    <div class="scroll-line">
                        <div class="scroll-dot"></div>
                    </div>
                </div>
            </div>

        </section>
    </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import { useRouter } from "vue-router";
import gsap from "gsap";

const router = useRouter();
const emit = defineEmits(["close"]);

const headerEl = ref(null);
const overlayEl = ref(null);
const containerEl = ref(null);
const displacementEl = ref(null);
const polygonEl = ref(null);

let isAnimating = false;
const clipPts = {
    x0: 0,
    y0: 0,

    x1: 1,
    y1: 0,

    x2: 1,
    y2: 1,

    x3: 0,
    y3: 1
};

// Touch Swipe variables
let touchStartY = 0;

const handleTouchStart = (e) => {
    touchStartY = e.touches[0].clientY;
};

const updatePolygon = () => {
    if (!polygonEl.value) return;

    polygonEl.value.setAttribute(
        "points",
        `
        ${clipPts.x0},${clipPts.y0}
        ${clipPts.x1},${clipPts.y1}
        ${clipPts.x2},${clipPts.y2}
        ${clipPts.x3},${clipPts.y3}
    `
    );
};

const handleTouchMove = (e) => {
    if (isAnimating) return;
    const touchEndY = e.touches[0].clientY;
    const diffY = touchStartY - touchEndY;
    // Swipe up (scroll down) triggers the transition
    if (diffY > 15) {
        runTransition();
    }
};

const runTransition = () => {
    if (isAnimating) return;
    isAnimating = true;

    // Ensure scroll is locked during transition
    document.body.style.overflow = "hidden";

    // Fade out overlay & header content quickly but smoothly
    gsap.to(
        [headerEl.value, overlayEl.value],
        {
            opacity: 0,
            duration: 0.5,
            ease: "power2.out"
        }
    );

    // Timeline for cloth pendulum swing and final drop
    const tl = gsap.timeline({
        onUpdate: updatePolygon,
        onComplete() {
            isAnimating = false;
            // Let parent component know we are done so it can unmount us
            emit("close");
            // Navigate to /about page to sync path
            router.replace("/about");
        }
    });

    // 1. WAVE DISTORTION: temporary ripple on release
    tl.to(displacementEl.value, {
        attr: { scale: 80 },
        duration: 0.2,
        ease: "power2.out"
    }, 0);

    // 2. STAGE 1: Release right corner -> Swing left (overshoot)
    // Easing sine.inOut makes the movement very smooth
    tl.to(clipPts, {
        x1: -0.15, y1: 0.85,
        x2: -0.5, y2: 1.7,
        x3: -0.05, y3: 1.05,
        duration: 1.4,
        ease: "sine.inOut"
    }, 0);

    // Content container rotates with the swing
    tl.to(containerEl.value, {
        rotation: 82,
        x: -60,
        y: 110,
        duration: 1.4,
        ease: "sine.inOut"
    }, 0);

    // 3. Swing back to the right
    tl.to(clipPts, {
        x1: 0.12, y1: 0.92,
        x2: 0.18, y2: 1.8,
        x3: 0.02, y3: 1.02,
        duration: 0.9,
        ease: "sine.inOut"
    });

    tl.to(containerEl.value, {
        rotation: 72,
        x: 10,
        y: 125,
        duration: 0.5,
        ease: "sine.inOut"
    }, "<");

    // 4. Settle hanging down from top-left
    tl.to(clipPts, {
        x1: 0.02, y1: 0.98,
        x2: 0.05, y2: 1.95,
        x3: 0.0, y3: 1.00,
        duration: 0.5,
        ease: "sine.inOut"
    });

    tl.to(containerEl.value, {
        rotation: 77,
        x: 0,
        y: 130,
        duration: 0.5,
        ease: "sine.inOut"
    }, "<");

    // Ripple settles down
    tl.to(displacementEl.value, {
        attr: { scale: 0 },
        duration: 0.1,
        ease: "sine.inOut"
    }, "<");

    // 5. STAGE 2: Release left corner -> drop completely out of view
    tl.to(clipPts, {
        x0: 0.0, y0: 2.2,
        x1: 0.0, y1: 2.3,
        x2: 0.0, y2: 2.4,
        x3: 0.0, y3: 2.2,
        duration: 1.3,
        ease: "power2.inOut"
    }, "+=0.1");

    tl.to(containerEl.value, {
        y: window.innerHeight * 1.6,
        rotation: 105,
        opacity: 0,
        duration: 1.3,
        ease: "power2.inOut"
    }, "<");
};

const handleWheel = (e) => {
    if (e.deltaY > 15 && !isAnimating) {
        runTransition();
    }
};

onMounted(() => {
    updatePolygon();
    setTimeout(() => {
        document.body.style.overflow = "hidden";
    }, 50);
    window.addEventListener("wheel", handleWheel, { passive: true });
    window.addEventListener("touchstart", handleTouchStart, { passive: true });
    window.addEventListener("touchmove", handleTouchMove, { passive: true });
});

onUnmounted(() => {
    document.body.style.overflow = "auto";
    window.removeEventListener("wheel", handleWheel);
    window.removeEventListener("touchstart", handleTouchStart);
    window.removeEventListener("touchmove", handleTouchMove);
});
</script>

<style scoped>
.hero-wrapper {
    position: fixed;
    inset: 0;
    z-index: 9999;
    width: 100vw;
    height: 100vh;
    pointer-events: none;
    filter: drop-shadow(0 25px 45px rgba(0, 0, 0, 0.4)) url(#wave-filter);
    will-change: transform, filter;
}

.hero-section {
    position: relative;
    width: 100%;
    height: 100%;
    overflow: hidden;
    background: #ffffff;
    /* White background */
    display: flex;
    justify-content: center;
    align-items: center;
    user-select: none;
    pointer-events: auto;
}

/* Header */
.header {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    padding: 40px 50px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    z-index: 20;
}

.logo img {
    height: 24px;
    display: block;
    filter: invert(1);
    /* invert to black for white background */
}

.menu-btn {
    width: 30px;
    height: 20px;
    border: none;
    background: transparent;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    cursor: pointer;
    padding: 0;
}

.menu-btn span {
    width: 100%;
    height: 2px;
    background: #000;
    display: block;
}

/* Hero Content */
.hero-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    filter: url(#wave-filter);
    will-change: transform, filter;
    transform-origin: top left;
    backface-visibility: hidden;
}

.hero-row-top {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 40px;
}

.text-make {
    font-family: 'Inter', sans-serif;
    font-weight: 900;
    font-size: clamp(60px, 12vw, 190px);
    line-height: 1;
    color: #000000;
    letter-spacing: -0.04em;
}

.logo-tagline-container {
    display: flex;
    align-items: center;
    gap: 25px;
}

.hero-logo {
    height: clamp(40px, 7vw, 108px);
    display: block;
    filter: invert(1);
}

.tagline-text {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;
    text-align: left;
    font-family: 'Inter', sans-serif;
    font-weight: 500;
    font-size: clamp(10px, 1.6vw, 24px);
    color: #000000;
    line-height: 1.45;
    letter-spacing: 0.02em;
}

.tagline-text strong {
    font-weight: 900;
}

.hero-row-bottom {
    margin-top: -2vw;
}

.text-awesome {
    font-family: 'Caveat Brush', cursive;
    font-size: clamp(90px, 18vw, 275px);
    line-height: 0.85;
    color: #000000;
    letter-spacing: -0.02em;
}

/* Overlay */
.overlay {
    position: absolute;
    inset: 0;
    z-index: 15;
    pointer-events: none;
}

.scroll-indicator {
    position: absolute;
    right: 50px;
    bottom: 40px;
    display: flex;
    flex-direction: column;
    align-items: center;
    color: #000;
}

.scroll-indicator span {
    font-size: 10px;
    letter-spacing: .25em;
    writing-mode: vertical-rl;
    margin-bottom: 12px;
}

.scroll-line {
    width: 1px;
    height: 90px;
    background: rgba(0, 0, 0, .2);
    overflow: hidden;
    position: relative;
}

.scroll-dot {
    position: absolute;
    inset: 0;
    background: #000;
    animation: scrollMove 2s cubic-bezier(.77, 0, .175, 1) infinite;
}

@keyframes scrollMove {
    0% {
        transform: translateY(-100%);
    }

    50% {
        transform: translateY(0%);
    }

    100% {
        transform: translateY(100%);
    }
}

.hidden-svg {
    position: absolute;
    width: 0;
    height: 0;
    overflow: hidden;
}

/* Mobile */
@media (max-width: 768px) {
    .header {
        padding: 24px;
    }

    .scroll-indicator {
        right: 24px;
        bottom: 24px;
    }

    .hero-row-top {
        gap: 20px;
    }

    .logo-tagline-container {
        gap: 15px;
    }
}
</style>
