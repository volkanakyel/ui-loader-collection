<template>
  <div class="min-h-screen bg-black p-8">
    <div class="max-w-6xl mx-auto">
      <h1 class="text-3xl font-light text-white text-center mb-2 tracking-wide">
        Loader Collection
      </h1>
      <p class="text-gray-500 text-center mb-8 text-sm">Click to preview</p>

      <!-- Filter Pills -->
      <div class="flex flex-wrap justify-center gap-2 mb-12 max-w-4xl mx-auto">
        <button
          v-for="category in categories"
          :key="category.id"
          @click="selectedCategory = category.id"
          :class="['filter-pill', selectedCategory === category.id ? 'filter-pill-active' : '']"
        >
          {{ category.label }}
        </button>
      </div>

      <div class="loader-grid-wrapper">
        <!-- Transition Morphism Overlay -->
        <Transition name="morphism">
          <div
            v-if="isTransitioning"
            class="transition-overlay"
          ></div>
        </Transition>

        <div
          class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-px bg-gradient-to-r from-transparent via-white/10 to-transparent"
        >
          <TransitionGroup name="loader-fade">
            <div
              v-for="(loader, index) in filteredLoaders"
              :key="loader.originalIndex"
              @click="openModal(loader.originalIndex)"
              class="loader-card group cursor-pointer"
              :style="{ '--stagger': index }"
            >
              <div class="loader-container">
                <div :class="`loader-${loader.originalIndex + 1}`"></div>
              </div>
              <p class="loader-title">{{ loader.name }}</p>
            </div>
          </TransitionGroup>
        </div>
      </div>

      <!-- Modal -->
      <Transition name="modal">
        <div
          v-if="selectedLoader !== null"
          class="modal-overlay"
          @click="closeModal"
        >
          <div
            class="modal-content"
            @click.stop
          >
            <button
              @click="closeModal"
              class="close-btn"
            >
              &times;
            </button>
            <h2 class="text-xl font-light text-white mb-12 tracking-wide">
              {{ loaders[selectedLoader].name }}
            </h2>
            <div class="flex justify-center items-center h-96">
              <div
                :class="`loader-${selectedLoader + 1}`"
                style="transform: scale(2.5)"
              ></div>
            </div>
          </div>
        </div>
      </Transition>
    </div>
  </div>
</template>

<script lang="ts" setup>
const selectedLoader = ref<number | null>(null)
const selectedCategory = ref<string>('all')
const isTransitioning = ref(false)
let transitionTimer: NodeJS.Timeout

const loaders = [
  { name: 'Radar Sweep', category: 'circular' },
  { name: 'Orbital Ring', category: 'circular' },
  { name: 'Morph Circle', category: 'circular' },
  { name: 'Pulse Ring', category: 'circular' },
  { name: 'Spinner', category: 'circular' },
  { name: 'Wave Ripple', category: 'circular' },
  { name: 'Dual Orbit', category: 'circular' },
  { name: 'Dots', category: 'circular' },
  { name: 'Arc Sweep', category: 'circular' },
  { name: 'Bars', category: 'linear' },
  { name: 'Square Corners', category: 'angular' },
  { name: 'Infinity Loop', category: 'linear' },
  { name: 'Cross Fade', category: 'linear' },
  { name: 'Concentric Spin', category: 'circular' },
  { name: 'Line Bounce', category: 'linear' },
  { name: 'Particle Swarm', category: 'angular' },
  { name: 'Ellipse Morph', category: 'circular' },
  { name: 'Grid Pulse', category: 'angular' },
  { name: 'Triangle Spin', category: 'angular' },
  { name: 'Square Rotate', category: 'angular' },
  { name: 'Zigzag', category: 'angular' },
  { name: 'Corner Dots', category: 'angular' },
  { name: 'Diamond Pulse', category: 'angular' },
  { name: 'Rectangle Slide', category: 'linear' },
  { name: 'Parallel Lines', category: 'linear' },
  { name: 'Staircase', category: 'angular' },
  { name: 'Arrow Path', category: 'angular' },
  { name: 'Hexagon Mesh', category: 'geometric' },
  { name: 'Pentagon Orbit', category: 'geometric' },
  { name: 'Octagon Spin', category: 'geometric' },
  { name: 'Tesseract', category: 'geometric' },
  { name: 'Kaleidoscope', category: 'geometric' },
  { name: 'Wave Flow', category: 'organic' },
  { name: 'Breathing', category: 'organic' },
  { name: 'Sine Pulse', category: 'organic' },
  { name: 'Elastic Band', category: 'organic' },
  { name: 'Glitch Matrix', category: 'digital' },
  { name: 'Binary Rain', category: 'digital' },
  { name: 'Pixel Fade', category: 'digital' },
  { name: 'Scan Line', category: 'digital' },
  { name: 'Fractal Spin', category: 'premium' },
  { name: 'Helix DNA', category: 'premium' },
  { name: 'Quantum Field', category: 'premium' },
  { name: 'Singularity', category: 'premium' },
]

const categories = [
  { id: 'all', label: 'All' },
  { id: 'circular', label: 'Circular' },
  { id: 'linear', label: 'Linear' },
  { id: 'angular', label: 'Angular' },
  { id: 'geometric', label: 'Geometric' },
  { id: 'organic', label: 'Organic' },
  { id: 'digital', label: 'Digital' },
  { id: 'premium', label: 'Premium' },
]

const filteredLoaders = computed(() => {
  if (selectedCategory.value === 'all') {
    return loaders.map((loader, index) => ({ ...loader, originalIndex: index }))
  }
  return loaders
    .map((loader, index) => ({ ...loader, originalIndex: index }))
    .filter(loader => loader.category === selectedCategory.value)
})

const openModal = (originalIndex: number) => {
  selectedLoader.value = originalIndex
}

const closeModal = () => {
  selectedLoader.value = null
}

// Handle transition state for morphism effect
watch(selectedCategory, () => {
  isTransitioning.value = true
  clearTimeout(transitionTimer)
  transitionTimer = setTimeout(() => {
    isTransitioning.value = false
  }, 1200) // Total transition time
})
</script>

<style>
/* Filter Pills */
.filter-pill {
  padding: 0.5rem 1.5rem;
  background: transparent;
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: #666;
  font-size: 0.75rem;
  font-weight: 300;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  cursor: pointer;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
}

.filter-pill::before {
  content: '';
  position: absolute;
  inset: 0;
  background: rgba(255, 255, 255, 0.05);
  transform: translateY(100%);
  transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.filter-pill:hover {
  border-color: rgba(255, 255, 255, 0.2);
  color: #999;
}

.filter-pill-active {
  border-color: rgba(255, 255, 255, 0.3);
  color: #fff;
}

.filter-pill-active::before {
  transform: translateY(0);
}

/* Loader Grid Wrapper */
.loader-grid-wrapper {
  position: relative;
  min-height: 400px;
}

/* Transition Morphism Overlay */
.transition-overlay {
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at center, rgba(255, 255, 255, 0.02), transparent);
  backdrop-filter: blur(20px);
  pointer-events: none;
  z-index: 10;
}

.morphism-enter-active,
.morphism-leave-active {
  transition: all 0.6s cubic-bezier(0.4, 0, 0.2, 1);
}

.morphism-enter-from,
.morphism-leave-to {
  opacity: 0;
  backdrop-filter: blur(0px);
}

.morphism-enter-to,
.morphism-leave-from {
  opacity: 1;
  backdrop-filter: blur(20px);
}

/* Loader Fade Transition */
.loader-fade-move {
  transition: all 0.8s cubic-bezier(0.4, 0, 0.2, 1);
}

.loader-fade-enter-active {
  transition: all 0.6s cubic-bezier(0.34, 1.56, 0.64, 1);
  transition-delay: calc(0.35s + var(--stagger, 0) * 0.025s);
}

.loader-fade-leave-active {
  transition: all 0.35s cubic-bezier(0.4, 0, 1, 0.4);
  position: absolute;
}

.loader-fade-enter-from {
  opacity: 0;
  transform: translateY(30px) scale(0.88);
  filter: blur(12px);
}

.loader-fade-leave-to {
  opacity: 0;
  transform: translateY(-20px) scale(0.85);
  filter: blur(15px);
}

/* Morphism effects during transition */
.loader-fade-enter-active,
.loader-fade-leave-active {
  will-change: transform, opacity, filter;
}

.loader-fade-enter-from .loader-card,
.loader-fade-leave-to .loader-card {
  background: rgba(255, 255, 255, 0.01);
  backdrop-filter: blur(8px);
  border-color: rgba(255, 255, 255, 0.05);
}

/* Smooth repositioning */
.loader-fade-move {
  z-index: 1;
}

/* Card Styles */
.loader-card {
  background: #000;
  padding: 3rem 2rem;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  border: 1px solid transparent;
}

.loader-card::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 10%;
  right: 10%;
  height: 1px;
  background: linear-gradient(to right, transparent, rgba(255, 255, 255, 0.1), transparent);
  transition: all 0.4s ease;
}

.loader-card:hover {
  border-color: rgba(255, 255, 255, 0.08);
  background: rgba(255, 255, 255, 0.01);
}

.loader-card:hover::after {
  background: linear-gradient(to right, transparent, rgba(255, 255, 255, 0.15), transparent);
}

.loader-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 120px;
  margin-bottom: 1.5rem;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.loader-fade-enter-from .loader-container,
.loader-fade-leave-to .loader-container {
  transform: scale(0.8);
  opacity: 0;
}

.loader-title {
  text-align: center;
  color: #666;
  font-size: 0.75rem;
  font-weight: 300;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.loader-card:hover .loader-title {
  color: #999;
  letter-spacing: 0.12em;
}

.loader-fade-enter-from .loader-title,
.loader-fade-leave-to .loader-title {
  opacity: 0;
  transform: translateY(10px);
}

/* Modal Styles */
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.95);
  backdrop-filter: blur(4px);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 50;
}

.modal-content {
  background: #000;
  border: 1px solid rgba(255, 255, 255, 0.1);
  padding: 3rem;
  max-width: 600px;
  width: 90%;
  position: relative;
}

.close-btn {
  position: absolute;
  top: 1.5rem;
  right: 1.5rem;
  background: transparent;
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: white;
  font-size: 1.5rem;
  width: 2.5rem;
  height: 2.5rem;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 200;
}

.close-btn:hover {
  background: rgba(255, 255, 255, 0.05);
  border-color: rgba(255, 255, 255, 0.2);
}

.modal-enter-active,
.modal-leave-active {
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.modal-enter-active .modal-content,
.modal-leave-active .modal-content {
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-content {
  transform: scale(0.95) translateY(20px);
  opacity: 0;
}

.modal-leave-to .modal-content {
  transform: scale(0.95) translateY(-20px);
  opacity: 0;
}

/* Loader 1: Radar Sweep */
.loader-1 {
  width: 64px;
  height: 64px;
  position: relative;
  animation: rotation 2s linear infinite;
}

.loader-1::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 50%;
  height: 2px;
  background: linear-gradient(to right, rgba(255, 255, 255, 0.8), transparent);
  transform-origin: left center;
}

/* Loader 2: Orbital Ring */
.loader-2 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-2::before {
  content: '';
  position: absolute;
  width: 6px;
  height: 6px;
  background: #fff;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 50%;
}

.loader-2::after {
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  border: 2px solid transparent;
  border-top-color: #fff;
  border-radius: 50%;
  animation: rotation 1.5s linear infinite;
}

/* Loader 3: Morph Circle */
.loader-3 {
  width: 64px;
  height: 64px;
  border: 2px solid #fff;
  animation: morph 3s ease-in-out infinite;
}

@keyframes morph {
  0%,
  100% {
    border-radius: 50%;
  }
  33% {
    border-radius: 0%;
  }
  66% {
    border-radius: 50%;
    transform: rotate(180deg);
  }
}

/* Loader 4: Pulse Ring */
.loader-4 {
  width: 64px;
  height: 64px;
  border: 2px solid #fff;
  border-radius: 50%;
  position: relative;
  animation: pulse 2s ease-in-out infinite;
}

@keyframes pulse {
  0%,
  100% {
    transform: scale(1);
    opacity: 1;
  }
  50% {
    transform: scale(1.3);
    opacity: 0.3;
  }
}

/* Loader 5: Spinner */
.loader-5 {
  width: 64px;
  height: 64px;
  border: 2px solid rgba(255, 255, 255, 0.1);
  border-top-color: #fff;
  border-radius: 50%;
  animation: rotation 1s linear infinite;
}

/* Loader 6: Wave Ripple */
.loader-6 {
  width: 64px;
  height: 64px;
  border-radius: 50%;
  position: relative;
}

.loader-6::before,
.loader-6::after {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: 50%;
  border: 2px solid rgba(255, 255, 255, 0.6);
  animation: ripple 2s ease-out infinite;
}

.loader-6::after {
  animation-delay: 1s;
}

@keyframes ripple {
  0% {
    transform: scale(0.5);
    opacity: 1;
  }
  100% {
    transform: scale(1.3);
    opacity: 0;
  }
}

/* Loader 7: Dual Orbit */
.loader-7 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-7::before,
.loader-7::after {
  content: '';
  position: absolute;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: #fff;
  top: 50%;
  left: 50%;
  margin: -5px;
}

.loader-7::before {
  animation: orbit1 2s linear infinite;
}

.loader-7::after {
  background: rgba(255, 255, 255, 0.4);
  animation: orbit2 2s linear infinite;
}

@keyframes orbit1 {
  0% {
    transform: translateX(-24px);
  }
  50% {
    transform: translateX(24px);
  }
  100% {
    transform: translateX(-24px);
  }
}

@keyframes orbit2 {
  0% {
    transform: translateX(24px);
  }
  50% {
    transform: translateX(-24px);
  }
  100% {
    transform: translateX(24px);
  }
}

/* Loader 8: Dots */
.loader-8 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-8::before,
.loader-8::after {
  content: '';
  position: absolute;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #fff;
  box-shadow: 24px 0 0 rgba(255, 255, 255, 0.6), -24px 0 0 rgba(255, 255, 255, 0.3),
    0 24px 0 rgba(255, 255, 255, 0.5), 0 -24px 0 rgba(255, 255, 255, 0.4);
  top: 28px;
  left: 28px;
  animation: rotation 3s linear infinite;
}

/* Loader 9: Arc Sweep */
.loader-9 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-9::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: 50%;
  background: conic-gradient(
    from 0deg,
    transparent 0deg 60deg,
    rgba(255, 255, 255, 0.8) 60deg 120deg,
    transparent 120deg 360deg
  );
  animation: rotation 1.5s linear infinite;
}

.loader-9::after {
  content: '';
  position: absolute;
  inset: 6px;
  background: #000;
  border-radius: 50%;
}

@keyframes rotation {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

/* Loader 10: Bars */
.loader-10 {
  width: 64px;
  height: 64px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 6px;
}

.loader-10::before,
.loader-10::after {
  content: '';
  width: 4px;
  height: 100%;
  background: #fff;
  animation: bars 1.2s ease-in-out infinite;
}

.loader-10::after {
  animation-delay: 0.4s;
}

@keyframes bars {
  0%,
  100% {
    transform: scaleY(0.3);
    opacity: 0.3;
  }
  50% {
    transform: scaleY(1);
    opacity: 1;
  }
}

/* Loader 11: Square Corners */
.loader-11 {
  width: 64px;
  height: 64px;
  position: relative;
  animation: rotation 2s linear infinite;
}

.loader-11::before,
.loader-11::after {
  content: '';
  position: absolute;
  width: 8px;
  height: 8px;
  background: #fff;
  border-radius: 50%;
}

.loader-11::before {
  top: 0;
  left: 0;
}

.loader-11::after {
  top: 0;
  right: 0;
  opacity: 0.5;
}

/* Loader 12: Infinity Loop */
.loader-12 {
  width: 80px;
  height: 40px;
  position: relative;
}

.loader-12::before {
  content: '';
  position: absolute;
  width: 12px;
  height: 12px;
  background: #fff;
  border-radius: 50%;
  animation: infinity 2s ease-in-out infinite;
}

@keyframes infinity {
  0% {
    top: 0;
    left: 0;
  }
  25% {
    top: 14px;
    left: 20px;
  }
  50% {
    top: 0;
    left: 34px;
  }
  75% {
    top: 14px;
    left: 48px;
  }
  100% {
    top: 0;
    left: 68px;
  }
}

/* Loader 13: Cross Fade */
.loader-13 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-13::before,
.loader-13::after {
  content: '';
  position: absolute;
  background: #fff;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.loader-13::before {
  width: 40px;
  height: 2px;
  animation: crossFade1 2s ease-in-out infinite;
}

.loader-13::after {
  width: 2px;
  height: 40px;
  animation: crossFade2 2s ease-in-out infinite;
}

@keyframes crossFade1 {
  0%,
  100% {
    opacity: 1;
  }
  50% {
    opacity: 0.2;
  }
}

@keyframes crossFade2 {
  0%,
  100% {
    opacity: 0.2;
  }
  50% {
    opacity: 1;
  }
}

/* Loader 14: Concentric Spin */
.loader-14 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-14::before,
.loader-14::after {
  content: '';
  position: absolute;
  border: 2px solid transparent;
  border-radius: 50%;
}

.loader-14::before {
  inset: 0;
  border-top-color: #fff;
  animation: rotation 1s linear infinite;
}

.loader-14::after {
  inset: 10px;
  border-top-color: rgba(255, 255, 255, 0.5);
  animation: rotation 1.5s linear infinite reverse;
}

/* Loader 15: Line Bounce */
.loader-15 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-15::before {
  content: '';
  position: absolute;
  width: 40px;
  height: 2px;
  background: #fff;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  animation: lineBounce 1.5s ease-in-out infinite;
}

@keyframes lineBounce {
  0%,
  100% {
    width: 40px;
    opacity: 1;
  }
  50% {
    width: 60px;
    opacity: 0.5;
  }
}

/* Loader 16: Particle Swarm */
.loader-16 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-16::before,
.loader-16::after {
  content: '';
  position: absolute;
  width: 6px;
  height: 6px;
  background: #fff;
  border-radius: 50%;
  top: 50%;
  left: 50%;
  margin: -3px;
}

.loader-16::before {
  animation: swarm1 2s ease-in-out infinite;
}

.loader-16::after {
  background: rgba(255, 255, 255, 0.6);
  animation: swarm2 2s ease-in-out infinite;
}

@keyframes swarm1 {
  0%,
  100% {
    transform: translate(-16px, -16px);
  }
  25% {
    transform: translate(16px, -16px);
  }
  50% {
    transform: translate(16px, 16px);
  }
  75% {
    transform: translate(-16px, 16px);
  }
}

@keyframes swarm2 {
  0%,
  100% {
    transform: translate(16px, 16px);
  }
  25% {
    transform: translate(-16px, 16px);
  }
  50% {
    transform: translate(-16px, -16px);
  }
  75% {
    transform: translate(16px, -16px);
  }
}

/* Loader 17: Ellipse Morph */
.loader-17 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-17::before {
  content: '';
  position: absolute;
  inset: 0;
  border: 2px solid #fff;
  animation: ellipseMorph 3s ease-in-out infinite;
}

@keyframes ellipseMorph {
  0%,
  100% {
    border-radius: 50%;
    transform: rotate(0deg);
  }
  33% {
    border-radius: 50% / 30%;
    transform: rotate(120deg);
  }
  66% {
    border-radius: 30% / 50%;
    transform: rotate(240deg);
  }
}

/* Loader 18: Grid Pulse */
.loader-18 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-18::before,
.loader-18::after {
  content: '';
  position: absolute;
  width: 4px;
  height: 4px;
  background: #fff;
  border-radius: 50%;
  box-shadow: 16px 0 0 rgba(255, 255, 255, 0.8), 32px 0 0 rgba(255, 255, 255, 0.6),
    0 16px 0 rgba(255, 255, 255, 0.7), 16px 16px 0 rgba(255, 255, 255, 0.5),
    32px 16px 0 rgba(255, 255, 255, 0.4), 0 32px 0 rgba(255, 255, 255, 0.6),
    16px 32px 0 rgba(255, 255, 255, 0.4), 32px 32px 0 rgba(255, 255, 255, 0.3);
  top: 8px;
  left: 8px;
  animation: gridPulse 2s ease-in-out infinite;
}

@keyframes gridPulse {
  0%,
  100% {
    opacity: 1;
  }
  50% {
    opacity: 0.2;
  }
}

/* Loader 19: Triangle Spin */
.loader-19 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-19::before {
  content: '';
  position: absolute;
  inset: 16px;
  border-left: 2px solid #fff;
  border-right: 2px solid #fff;
  border-bottom: 2px solid #fff;
  clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
  animation: rotation 2s linear infinite;
}

/* Loader 20: Square Rotate */
.loader-20 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-20::before {
  content: '';
  position: absolute;
  inset: 16px;
  border: 2px solid #fff;
  animation: rotation 2s linear infinite;
}

/* Loader 21: Zigzag */
.loader-21 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-21::before {
  content: '';
  position: absolute;
  width: 8px;
  height: 8px;
  background: #fff;
  animation: zigzag 2s ease-in-out infinite;
}

@keyframes zigzag {
  0% {
    top: 0;
    left: 0;
  }
  25% {
    top: 18px;
    left: 18px;
  }
  50% {
    top: 36px;
    left: 0;
  }
  75% {
    top: 54px;
    left: 18px;
  }
  100% {
    top: 0;
    left: 0;
  }
}

/* Loader 22: Corner Dots */
.loader-22 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-22::before,
.loader-22::after {
  content: '';
  position: absolute;
  width: 6px;
  height: 6px;
  background: #fff;
}

.loader-22::before {
  animation: cornerDots1 2s ease-in-out infinite;
}

.loader-22::after {
  background: rgba(255, 255, 255, 0.5);
  animation: cornerDots2 2s ease-in-out infinite;
}

@keyframes cornerDots1 {
  0%,
  100% {
    top: 0;
    left: 0;
  }
  25% {
    top: 0;
    left: 58px;
  }
  50% {
    top: 58px;
    left: 58px;
  }
  75% {
    top: 58px;
    left: 0;
  }
}

@keyframes cornerDots2 {
  0%,
  100% {
    top: 0;
    left: 58px;
  }
  25% {
    top: 58px;
    left: 58px;
  }
  50% {
    top: 58px;
    left: 0;
  }
  75% {
    top: 0;
    left: 0;
  }
}

/* Loader 23: Diamond Pulse */
.loader-23 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-23::before {
  content: '';
  position: absolute;
  inset: 16px;
  border: 2px solid #fff;
  transform: rotate(45deg);
  animation: pulse 2s ease-in-out infinite;
}

/* Loader 24: Rectangle Slide */
.loader-24 {
  width: 64px;
  height: 64px;
  position: relative;
  overflow: hidden;
}

.loader-24::before {
  content: '';
  position: absolute;
  width: 24px;
  height: 8px;
  background: #fff;
  top: 28px;
  animation: rectSlide 1.5s ease-in-out infinite;
}

@keyframes rectSlide {
  0%,
  100% {
    left: 0;
  }
  50% {
    left: 40px;
  }
}

/* Loader 25: Parallel Lines */
.loader-25 {
  width: 64px;
  height: 64px;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.loader-25::before,
.loader-25::after {
  content: '';
  width: 2px;
  height: 40px;
  background: #fff;
}

.loader-25::before {
  animation: parallelLine1 1.5s ease-in-out infinite;
}

.loader-25::after {
  animation: parallelLine2 1.5s ease-in-out infinite;
}

@keyframes parallelLine1 {
  0%,
  100% {
    transform: translateY(0);
    opacity: 1;
  }
  50% {
    transform: translateY(-10px);
    opacity: 0.3;
  }
}

@keyframes parallelLine2 {
  0%,
  100% {
    transform: translateY(0);
    opacity: 0.3;
  }
  50% {
    transform: translateY(10px);
    opacity: 1;
  }
}

/* Loader 26: Staircase */
.loader-26 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-26::before {
  content: '';
  position: absolute;
  width: 8px;
  height: 8px;
  background: #fff;
  animation: staircase 2s ease-in-out infinite;
}

@keyframes staircase {
  0% {
    top: 0;
    left: 0;
  }
  16.66% {
    top: 0;
    left: 14px;
  }
  33.33% {
    top: 14px;
    left: 14px;
  }
  50% {
    top: 14px;
    left: 28px;
  }
  66.66% {
    top: 28px;
    left: 28px;
  }
  83.33% {
    top: 28px;
    left: 42px;
  }
  100% {
    top: 42px;
    left: 42px;
  }
}

/* Loader 27: Arrow Path */
.loader-27 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-27::before,
.loader-27::after {
  content: '';
  position: absolute;
  background: #fff;
  top: 50%;
  left: 50%;
}

.loader-27::before {
  width: 30px;
  height: 2px;
  transform: translate(-50%, -50%);
  animation: arrowShaft 2s ease-in-out infinite;
}

.loader-27::after {
  width: 0;
  height: 0;
  border-left: 6px solid #fff;
  border-top: 4px solid transparent;
  border-bottom: 4px solid transparent;
  transform: translate(-50%, -50%);
  animation: arrowHead 2s ease-in-out infinite;
}

@keyframes arrowShaft {
  0%,
  100% {
    width: 30px;
    opacity: 1;
  }
  50% {
    width: 40px;
    opacity: 0.5;
  }
}

@keyframes arrowHead {
  0%,
  100% {
    left: calc(50% + 15px);
  }
  50% {
    left: calc(50% + 20px);
  }
}

/* GEOMETRIC CATEGORY */

/* Loader 28: Hexagon Mesh */
.loader-28 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-28::before,
.loader-28::after {
  content: '';
  position: absolute;
  inset: 12px;
  clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
  border: 2px solid #fff;
}

.loader-28::before {
  animation: rotation 3s linear infinite;
}

.loader-28::after {
  inset: 18px;
  opacity: 0.5;
  animation: rotation 3s linear infinite reverse;
}

/* Loader 29: Pentagon Orbit */
.loader-29 {
  width: 64px;
  height: 64px;
  position: relative;
  animation: rotation 4s linear infinite;
}

.loader-29::before {
  content: '';
  position: absolute;
  inset: 14px;
  clip-path: polygon(50% 0%, 100% 38%, 82% 100%, 18% 100%, 0% 38%);
  border: 2px solid #fff;
}

.loader-29::after {
  content: '';
  position: absolute;
  width: 6px;
  height: 6px;
  background: #fff;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  border-radius: 50%;
}

/* Loader 30: Octagon Spin */
.loader-30 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-30::before {
  content: '';
  position: absolute;
  inset: 12px;
  clip-path: polygon(30% 0%, 70% 0%, 100% 30%, 100% 70%, 70% 100%, 30% 100%, 0% 70%, 0% 30%);
  border: 2px solid #fff;
  animation: rotation 2.5s ease-in-out infinite;
}

/* Loader 31: Tesseract */
.loader-31 {
  width: 64px;
  height: 64px;
  position: relative;
  perspective: 200px;
}

.loader-31::before,
.loader-31::after {
  content: '';
  position: absolute;
  border: 2px solid #fff;
}

.loader-31::before {
  inset: 8px;
  animation: tesseract1 3s ease-in-out infinite;
}

.loader-31::after {
  inset: 20px;
  animation: tesseract2 3s ease-in-out infinite;
}

@keyframes tesseract1 {
  0%,
  100% {
    transform: rotateY(0deg) rotateX(0deg);
  }
  50% {
    transform: rotateY(180deg) rotateX(180deg);
  }
}

@keyframes tesseract2 {
  0%,
  100% {
    transform: rotateY(180deg) rotateX(180deg);
  }
  50% {
    transform: rotateY(0deg) rotateX(0deg);
  }
}

/* Loader 32: Kaleidoscope */
.loader-32 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-32::before,
.loader-32::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 2px;
  height: 30px;
  background: #fff;
  transform-origin: 0 0;
}

.loader-32::before {
  animation: kaleidoscope1 3s linear infinite;
}

.loader-32::after {
  animation: kaleidoscope2 3s linear infinite;
}

@keyframes kaleidoscope1 {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

@keyframes kaleidoscope2 {
  0% {
    transform: rotate(0deg) scaleX(-1);
  }
  100% {
    transform: rotate(-360deg) scaleX(-1);
  }
}

/* ORGANIC CATEGORY */

/* Loader 33: Wave Flow */
.loader-33 {
  width: 64px;
  height: 64px;
  position: relative;
  overflow: hidden;
}

.loader-33::before {
  content: '';
  position: absolute;
  width: 100px;
  height: 2px;
  background: #fff;
  top: 50%;
  left: -20px;
  animation: waveFlow 2s ease-in-out infinite;
}

@keyframes waveFlow {
  0%,
  100% {
    transform: translateY(0);
  }
  25% {
    transform: translateY(-10px);
  }
  75% {
    transform: translateY(10px);
  }
}

/* Loader 34: Breathing */
.loader-34 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-34::before {
  content: '';
  position: absolute;
  inset: 16px;
  border: 2px solid #fff;
  border-radius: 50%;
  animation: breathing 3s ease-in-out infinite;
}

@keyframes breathing {
  0%,
  100% {
    transform: scale(1);
    opacity: 1;
  }
  50% {
    transform: scale(1.4);
    opacity: 0.4;
  }
}

/* Loader 35: Sine Pulse */
.loader-35 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-35::before,
.loader-35::after {
  content: '';
  position: absolute;
  width: 8px;
  height: 8px;
  background: #fff;
  border-radius: 50%;
  top: 50%;
  left: 50%;
}

.loader-35::before {
  animation: sine1 2s ease-in-out infinite;
}

.loader-35::after {
  opacity: 0.5;
  animation: sine2 2s ease-in-out infinite;
}

@keyframes sine1 {
  0%,
  100% {
    transform: translate(-20px, 0);
  }
  25% {
    transform: translate(-10px, -15px);
  }
  50% {
    transform: translate(0, 0);
  }
  75% {
    transform: translate(10px, 15px);
  }
}

@keyframes sine2 {
  0%,
  100% {
    transform: translate(20px, 0);
  }
  25% {
    transform: translate(10px, 15px);
  }
  50% {
    transform: translate(0, 0);
  }
  75% {
    transform: translate(-10px, -15px);
  }
}

/* Loader 36: Elastic Band */
.loader-36 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-36::before {
  content: '';
  position: absolute;
  width: 40px;
  height: 2px;
  background: #fff;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  animation: elastic 2s cubic-bezier(0.68, -0.55, 0.265, 1.55) infinite;
}

@keyframes elastic {
  0%,
  100% {
    width: 40px;
    height: 2px;
  }
  50% {
    width: 2px;
    height: 40px;
  }
}

/* DIGITAL CATEGORY */

/* Loader 37: Glitch Matrix */
.loader-37 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-37::before,
.loader-37::after {
  content: '';
  position: absolute;
  width: 30px;
  height: 2px;
  background: #fff;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.loader-37::before {
  animation: glitch1 1s steps(4) infinite;
}

.loader-37::after {
  animation: glitch2 1s steps(4) infinite;
}

@keyframes glitch1 {
  0%,
  100% {
    transform: translate(-50%, -50%) translateX(0);
  }
  25% {
    transform: translate(-50%, -50%) translateX(-5px);
  }
  50% {
    transform: translate(-50%, -50%) translateX(5px);
  }
  75% {
    transform: translate(-50%, -50%) translateX(-3px);
  }
}

@keyframes glitch2 {
  0%,
  100% {
    transform: translate(-50%, -50%) translateY(0);
    opacity: 1;
  }
  25% {
    transform: translate(-50%, -50%) translateY(-5px);
    opacity: 0.7;
  }
  50% {
    transform: translate(-50%, -50%) translateY(5px);
    opacity: 0.3;
  }
  75% {
    transform: translate(-50%, -50%) translateY(-3px);
    opacity: 0.5;
  }
}

/* Loader 38: Binary Rain */
.loader-38 {
  width: 64px;
  height: 64px;
  position: relative;
  overflow: hidden;
}

.loader-38::before,
.loader-38::after {
  content: '';
  position: absolute;
  width: 2px;
  height: 20px;
  background: linear-gradient(to bottom, transparent, #fff);
  animation: binaryRain 1.5s linear infinite;
}

.loader-38::before {
  left: 20px;
}

.loader-38::after {
  left: 40px;
  animation-delay: 0.5s;
}

@keyframes binaryRain {
  0% {
    top: -20px;
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
  100% {
    top: 64px;
    opacity: 0;
  }
}

/* Loader 39: Pixel Fade */
.loader-39 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-39::before {
  content: '';
  position: absolute;
  inset: 20px;
  background: #fff;
  animation: pixelFade 2s steps(8) infinite;
}

@keyframes pixelFade {
  0%,
  100% {
    clip-path: inset(0 0 0 0);
    opacity: 1;
  }
  25% {
    clip-path: inset(0 0 50% 0);
    opacity: 0.7;
  }
  50% {
    clip-path: inset(0 0 100% 0);
    opacity: 0;
  }
  75% {
    clip-path: inset(50% 0 0 0);
    opacity: 0.7;
  }
}

/* Loader 40: Scan Line */
.loader-40 {
  width: 64px;
  height: 64px;
  position: relative;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.loader-40::before {
  content: '';
  position: absolute;
  width: 100%;
  height: 2px;
  background: #fff;
  top: 0;
  left: 0;
  animation: scanLine 2s linear infinite;
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
}

@keyframes scanLine {
  0% {
    top: 0;
  }
  100% {
    top: calc(100% - 2px);
  }
}

/* PREMIUM CATEGORY */

/* Loader 41: Fractal Spin */
.loader-41 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-41::before,
.loader-41::after {
  content: '';
  position: absolute;
  border: 2px solid #fff;
}

.loader-41::before {
  inset: 0;
  clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
  animation: fractal1 3s cubic-bezier(0.68, -0.55, 0.265, 1.55) infinite;
}

.loader-41::after {
  inset: 16px;
  clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
  animation: fractal2 3s cubic-bezier(0.68, -0.55, 0.265, 1.55) infinite;
}

@keyframes fractal1 {
  0%,
  100% {
    transform: rotate(0deg) scale(1);
  }
  50% {
    transform: rotate(180deg) scale(0.8);
  }
}

@keyframes fractal2 {
  0%,
  100% {
    transform: rotate(0deg) scale(1);
  }
  50% {
    transform: rotate(-180deg) scale(1.2);
  }
}

/* Loader 42: Helix DNA */
.loader-42 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-42::before,
.loader-42::after {
  content: '';
  position: absolute;
  width: 4px;
  height: 4px;
  background: #fff;
  border-radius: 50%;
  top: 50%;
  left: 50%;
  margin: -2px;
  box-shadow: 0 -20px 0 #fff, 0 20px 0 #fff;
}

.loader-42::before {
  animation: helix1 2s ease-in-out infinite;
}

.loader-42::after {
  animation: helix2 2s ease-in-out infinite;
}

@keyframes helix1 {
  0%,
  100% {
    transform: translateX(-16px) rotateY(0deg);
    opacity: 1;
  }
  50% {
    transform: translateX(16px) rotateY(180deg);
    opacity: 0.3;
  }
}

@keyframes helix2 {
  0%,
  100% {
    transform: translateX(16px) rotateY(0deg);
    opacity: 1;
  }
  50% {
    transform: translateX(-16px) rotateY(180deg);
    opacity: 0.3;
  }
}

/* Loader 43: Quantum Field */
.loader-43 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-43::before,
.loader-43::after {
  content: '';
  position: absolute;
  inset: 0;
  border: 1px solid #fff;
  border-radius: 50%;
}

.loader-43::before {
  animation: quantum1 3s cubic-bezier(0.785, 0.135, 0.15, 0.86) infinite;
}

.loader-43::after {
  animation: quantum2 3s cubic-bezier(0.785, 0.135, 0.15, 0.86) infinite;
}

@keyframes quantum1 {
  0%,
  100% {
    transform: scale(1) rotate(0deg);
    border-radius: 50%;
    opacity: 1;
  }
  25% {
    transform: scale(0.5) rotate(90deg);
    border-radius: 0%;
    opacity: 0.5;
  }
  50% {
    transform: scale(1) rotate(180deg);
    border-radius: 50%;
    opacity: 1;
  }
  75% {
    transform: scale(0.5) rotate(270deg);
    border-radius: 0%;
    opacity: 0.5;
  }
}

@keyframes quantum2 {
  0%,
  100% {
    transform: scale(0.5) rotate(180deg);
    border-radius: 0%;
    opacity: 0.5;
  }
  25% {
    transform: scale(1) rotate(270deg);
    border-radius: 50%;
    opacity: 1;
  }
  50% {
    transform: scale(0.5) rotate(360deg);
    border-radius: 0%;
    opacity: 0.5;
  }
  75% {
    transform: scale(1) rotate(450deg);
    border-radius: 50%;
    opacity: 1;
  }
}

/* Loader 44: Singularity */
.loader-44 {
  width: 64px;
  height: 64px;
  position: relative;
}

.loader-44::before,
.loader-44::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.loader-44::before {
  width: 2px;
  height: 2px;
  background: #fff;
  border-radius: 50%;
  box-shadow: 0 0 0 0 rgba(255, 255, 255, 0.7), 0 0 0 10px rgba(255, 255, 255, 0.4),
    0 0 0 20px rgba(255, 255, 255, 0.2), 0 0 0 30px rgba(255, 255, 255, 0.1);
  animation: singularity 3s ease-in-out infinite;
}

@keyframes singularity {
  0%,
  100% {
    box-shadow: 0 0 0 0 rgba(255, 255, 255, 1), 0 0 0 10px rgba(255, 255, 255, 0),
      0 0 0 20px rgba(255, 255, 255, 0), 0 0 0 30px rgba(255, 255, 255, 0);
  }
  50% {
    box-shadow: 0 0 0 0 rgba(255, 255, 255, 1), 0 0 0 10px rgba(255, 255, 255, 0.7),
      0 0 0 20px rgba(255, 255, 255, 0.4), 0 0 0 30px rgba(255, 255, 255, 0.1);
  }
}
</style>
