<template>
    <div class="hero-container">
        <!-- ── LAYER 0: Dark page behind (revealed by wave) ── -->
        <div class="below-panel">
            <div class="below-content">
                <!-- ── LAYER 1: Hero (single panel with animated wave clip-path) ── -->
                <div class="hero-panel" ref="heroPanel">

                    <header class="header" ref="headerEl">
                        <div class="logo"><img src="/logo.svg" alt="Kirifuda" /></div>
                        <div class="menu-btn"><span></span><span></span></div>
                    </header>

                    <div class="hero-image-wrap">
                        <img src="/title_01.png" class="hero-img" alt="Make AWESOME" @load="onImageLoad" />
                    </div>

                    <div class="content-overlay" ref="overlayEl">
                        <p class="tagline">CRAFTING DIGITAL EXPERIENCES THROUGH DESIGN AND TECHNOLOGY.</p>
                        <div class="scroll-indicator position-fixd">
                            <p>SCROLL</p>
                            <div class="scroll-line">
                                <div class="scroll-dot"></div>
                            </div>
                        </div>
                    </div>

                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import { useRouter } from 'vue-router';
import gsap from 'gsap';

// import ScrollTrigger from "gsap/ScrollTrigger";

// gsap.registerPlugin(ScrollTrigger);

const router = useRouter();
const heroPanel = ref(null);
const headerEl = ref(null);
const overlayEl = ref(null);
const isLoading = ref(true);
const isAnimating = ref(false);

const onImageLoad = () => setTimeout(() => { isLoading.value = false; }, 2000);


const TOP_XS = [0, 12.5, 25, 37.5, 50, 62.5, 75, 87.5, 100];

const runWave = () => {
    if (isAnimating.value) return;
    isAnimating.value = true;

    // State object we'll update each frame to build the polygon string
    const pts = { y0: 0, y1: 0, y2: 0, y3: 0, y4: 0, y5: 0, y6: 0, y7: 0, y8: 0 };

    const updateClip = () => {
        // Build polygon: top edge (9 points left→right) + bottom-right + bottom-left
        const top = TOP_XS.map((x, i) => `${x}% ${pts['y' + i]}%`).join(', ');
        heroPanel.value.style.clipPath = `polygon(${top}, 100% 100%, 0% 100%)`;
    };

    const tl = gsap.timeline({ onComplete: () => router.push('/about') });

    // Fade header + overlay first
    tl.to([headerEl.value, overlayEl.value], {
        opacity: 0, duration: 0.3, ease: 'power2.out'
    }, 0);


    TOP_XS.forEach((_, i) => {
        const key = 'y' + i;
        const rightIndex = 8 - i;         // flip: rightmost (i=8) starts first
        const startAt = 0.1 + rightIndex * 0.09; // 0.1s → 0.82s stagger from right→left
        const dur = 1.0 + i * 0.07;           // left points fall slightly longer (gravity)

        tl.to(pts, {
            [key]: 200,
            duration: dur,
            ease: 'power3.in',
            onUpdate: updateClip
        }, startAt);
    });
};

// ─── Wheel + Touch ───────────────────────────────────────────────────
const handleWheel = (e) => {
    if (e.deltaY > 20) runWave();
};

let _tsY = 0;
const handleTouchStart = (e) => { _tsY = e.touches[0].clientY; };
const handleTouchEnd = (e) => {
    if (_tsY - e.changedTouches[0].clientY > 40) runWave();
};

onMounted(() => {
    // Start flat
    heroPanel.value.style.clipPath =
        `polygon(${TOP_XS.map(x => `${x}% 0%`).join(', ')}, 100% 100%, 0% 100%)`;

    window.addEventListener('wheel', handleWheel, { passive: true });
    window.addEventListener('touchstart', handleTouchStart, { passive: true });
    window.addEventListener('touchend', handleTouchEnd, { passive: true });
});

onUnmounted(() => {
    window.removeEventListener('wheel', handleWheel);
    window.removeEventListener('touchstart', handleTouchStart);
    window.removeEventListener('touchend', handleTouchEnd);
    gsap.killTweensOf([heroPanel.value, headerEl.value, overlayEl.value]);
});
</script>

<style scoped>
.hero-container {
    position: fixed;
    inset: 0;
    width: 100vw;
    height: 100vh;
    overflow: hidden;
}

/* Preloader */
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

@keyframes pulse {

    0%,
    100% {
        transform: scale(0.9);
        opacity: 0.5;
    }

    50% {
        transform: scale(1);
        opacity: 1;
    }
}

/* Dark behind page */
.below-panel {
    position: absolute;
    inset: 0;
    background: #0d0d0d;
    z-index: 0;
    display: flex;
    align-items: center;
    justify-content: center;
}

.below-content {
    text-align: center;
    color: #fff;
}

.below-content h2 {
    font-size: 8vw;
    font-weight: 700;
    letter-spacing: 0.05em;
    margin-bottom: 16px;
}

.below-content p {
    font-size: 1rem;
    letter-spacing: 0.2em;
    opacity: 0.55;
    margin-bottom: 32px;
}

.about-link {
    color: #fff;
    border: 1px solid rgba(255, 255, 255, 0.4);
    padding: 12px 28px;
    text-decoration: none;
    font-size: 12px;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    transition: background 0.3s, color 0.3s;
}

.about-link:hover {
    background: #fff;
    color: #000;
}

/* Single hero panel — clip-path updated by GSAP */
.hero-panel {
    position: absolute;
    inset: 0;
    background: #fff;
    z-index: 10;
    will-change: clip-path;
}

/* Make AWESOME image */
.hero-image-wrap {
    position: absolute;
    inset: 0;
    display: flex;
    align-items: center;
    justify-content: center;
}

.hero-img {
    width: 90%;
    max-width: 1100px;
    object-fit: contain;
    display: block;
    user-select: none;
    pointer-events: none;
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
}

.menu-btn {
    width: 30px;
    height: 20px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    cursor: pointer;
}

.menu-btn span {
    display: block;
    width: 100%;
    height: 2px;
    background: #000;
}

/* Content overlay */
.content-overlay {
    position: absolute;
    inset: 0;
    z-index: 15;
    pointer-events: none;
}

.tagline {
    position: absolute;
    bottom: 50px;
    left: 50px;
    color: #000;
    font-size: 11px;
    letter-spacing: 0.1em;
    font-weight: 500;
    max-width: 250px;
}

.scroll-indicator {
    position: absolute;
    bottom: 0;
    right: 50px;
    display: flex;
    flex-direction: column;
    align-items: center;
    color: #000;
}

.scroll-indicator p {
    font-size: 10px;
    letter-spacing: 0.2em;
    writing-mode: vertical-rl;
    margin-bottom: 12px;
}

.scroll-line {
    width: 1px;
    height: 80px;
    background: rgba(0, 0, 0, 0.2);
    position: relative;
    overflow: hidden;
}

.scroll-dot {
    position: absolute;
    top: -100%;
    left: 0;
    width: 100%;
    height: 100%;
    background: #000;
    animation: scrollDown 2s cubic-bezier(0.77, 0, 0.175, 1) infinite;
}

@keyframes scrollDown {
    0% {
        transform: translateY(-100%);
    }

    50% {
        transform: translateY(0);
    }

    100% {
        transform: translateY(100%);
    }
}

@media (max-width: 768px) {
    .header {
        padding: 25px;
    }

    .tagline {
        bottom: 25px;
        left: 25px;
    }

    .scroll-indicator {
        right: 25px;
    }
}
</style>
