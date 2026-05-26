<template>
  <div class="relative flex flex-col h-full text-white overflow-hidden app-bg">

    <!-- Animated background blobs -->
    <div class="bg-blob bg-blob-1"></div>
    <div class="bg-blob bg-blob-2"></div>
    <div class="bg-blob bg-blob-3"></div>

    <Transition name="slide-fade" mode="out-in">
      <!-- START SCREEN -->
      <div
        v-if="!started"
        key="start"
        class="relative z-10 flex-1 flex flex-col items-center justify-around px-5 py-6 text-center min-h-0"
      >
        <h1 class="title-glow font-extrabold leading-tight max-w-md">
          <span class="block text-xl sm:text-2xl text-white/85">
            Was ist jetzt eigentlich mit
          </span>
          <span class="block bg-clip-text text-transparent bg-gradient-to-r from-pink-400 via-fuchsia-400 to-purple-400 text-4xl sm:text-5xl mt-1">
            Mascha &amp; Cedi
          </span>
          <span class="block text-white/60 text-2xl font-medium mt-1">?</span>
        </h1>

        <!-- Floating bubbles -->
        <div class="flex items-center justify-center gap-2 sm:gap-4 w-full">
          <div class="bubble bubble-left">
            <img src="/images/cEDI.JPG" alt="Cedi" />
          </div>
          <div class="heart-pulse">💕</div>
          <div class="bubble bubble-right">
            <img src="/images/mASCHA.JPG" alt="Mascha" />
          </div>
        </div>

        <div class="flex flex-col items-center gap-5 w-full">
          <p class="text-white/80 text-lg sm:text-xl italic tracking-wide">
            Because we Like Quiz
          </p>

          <button
            class="start-btn w-full max-w-xs py-4 rounded-full text-white font-extrabold text-2xl tracking-[0.3em] active:scale-95 transition-transform"
            @click="start"
          >
            START
          </button>
        </div>
      </div>

      <!-- QUIZ / END SCREEN WRAPPER -->
      <div v-else key="quiz" class="relative z-10 flex-1 flex flex-col overflow-hidden">
        <!-- Progress bar (hidden on end screen) -->
        <ProgressBar
          v-if="!done"
          :current="currentIndex"
          :total="slides.length"
        />

        <Transition name="slide-fade" mode="out-in">
          <div v-if="!done" :key="currentIndex" class="flex-1 overflow-hidden">
            <SlideRenderer
              :slide="currentSlide"
              @answer="onAnswer"
              @jump="onJump"
            />
          </div>

          <div
            v-else
            key="end"
            class="flex-1 flex flex-col items-center justify-center px-8 text-center gap-6"
          >
            <div class="text-7xl">🎉</div>
            <h1 class="text-4xl font-extrabold text-white leading-tight">
              Fertig!
            </h1>
            <p class="text-white/60 text-xl">
              Das war unser kleines Quiz. 💕
            </p>
            <button
              class="mt-4 px-10 py-5 rounded-2xl bg-pink-500 text-white font-bold text-xl active:scale-95 transition-transform shadow-lg shadow-pink-500/30"
              @click="restart"
            >
              Nochmal spielen ↺
            </button>
          </div>
        </Transition>

        <div
          v-if="!done"
          class="absolute top-2 right-3 text-white/30 text-xs tabular-nums select-none z-20"
        >
          {{ currentIndex + 1 }} / {{ slides.length }}
        </div>
      </div>
    </Transition>

  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import quizData from './data/quiz.json'
import ProgressBar from './components/ProgressBar.vue'
import SlideRenderer from './components/SlideRenderer.vue'

const slides = quizData.slides
const currentIndex = ref(0)
const answers = ref([])
const done = ref(false)
const started = ref(false)

const currentSlide = computed(() => slides[currentIndex.value])

function start() {
  started.value = true
}

function onJump(type) {
  const idx = slides.findIndex(s => s.type === type)
  if (idx !== -1) currentIndex.value = idx
}

function onAnswer(value) {
  answers.value[currentIndex.value] = value
  if (currentIndex.value + 1 >= slides.length) {
    done.value = true
  } else {
    currentIndex.value++
  }
}

function restart() {
  currentIndex.value = 0
  answers.value = []
  done.value = false
  started.value = false
}
</script>

<style scoped>
.slide-fade-enter-active,
.slide-fade-leave-active {
  transition: opacity 0.4s ease, transform 0.4s ease;
}
.slide-fade-enter-from {
  opacity: 0;
  transform: translateX(30px);
}
.slide-fade-leave-to {
  opacity: 0;
  transform: translateX(-30px);
}

/* ---------- Background ---------- */
.app-bg {
  background:
    radial-gradient(ellipse at top, #2a0a3a 0%, #0a0612 55%, #050208 100%);
}

.bg-blob {
  position: absolute;
  border-radius: 9999px;
  filter: blur(80px);
  opacity: 0.55;
  pointer-events: none;
  z-index: 0;
}
.bg-blob-1 {
  width: 340px; height: 340px;
  background: #ec4899;
  top: -80px; left: -80px;
  animation: float 9s ease-in-out infinite;
}
.bg-blob-2 {
  width: 380px; height: 380px;
  background: #8b5cf6;
  bottom: -100px; right: -100px;
  animation: float 11s ease-in-out infinite reverse;
}
.bg-blob-3 {
  width: 260px; height: 260px;
  background: #06b6d4;
  top: 40%; left: 50%;
  transform: translate(-50%, -50%);
  opacity: 0.35;
  animation: float 13s ease-in-out infinite;
}

@keyframes float {
  0%, 100% { transform: translate(0, 0) scale(1); }
  50%      { transform: translate(20px, -30px) scale(1.08); }
}

/* ---------- Title ---------- */
.title-glow {
  text-shadow: 0 0 25px rgba(236, 72, 153, 0.35);
}

/* ---------- Bubbles ---------- */
.bubble {
  flex: 0 0 auto;
  width: clamp(120px, 38vw, 180px);
  height: clamp(120px, 38vw, 180px);
  border-radius: 9999px;
  overflow: hidden;
  border: 4px solid rgba(255, 255, 255, 0.92);
  box-shadow:
    0 0 0 4px rgba(236, 72, 153, 0.35),
    0 20px 60px -10px rgba(236, 72, 153, 0.6),
    inset 0 0 30px rgba(0, 0, 0, 0.25);
  background:
    radial-gradient(circle at 30% 30%, #2a0a3a 0%, #0a0612 100%);
  display: flex;
  align-items: center;
  justify-content: center;
}
.bubble img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center top;
  padding: 0;
  display: block;
}
.bubble-left {
  animation: bob 5s ease-in-out infinite;
}
.bubble-right {
  box-shadow:
    0 0 0 4px rgba(139, 92, 246, 0.4),
    0 20px 60px -10px rgba(139, 92, 246, 0.6),
    inset 0 0 30px rgba(0, 0, 0, 0.25);
  animation: bob 6s ease-in-out infinite reverse;
}

@keyframes bob {
  0%, 100% { transform: translateY(0) rotate(-2deg); }
  50%      { transform: translateY(-10px) rotate(2deg); }
}

.heart-pulse {
  font-size: clamp(1.75rem, 6vw, 2.5rem);
  filter: drop-shadow(0 0 12px rgba(236, 72, 153, 0.8));
  animation: heartbeat 1.4s ease-in-out infinite;
  flex: 0 0 auto;
}

@keyframes heartbeat {
  0%, 100% { transform: scale(1); }
  25%      { transform: scale(1.25); }
  50%      { transform: scale(1); }
  75%      { transform: scale(1.15); }
}

/* ---------- Start button ---------- */
.start-btn {
  background: linear-gradient(135deg, #ec4899 0%, #a855f7 50%, #6366f1 100%);
  background-size: 200% 200%;
  box-shadow:
    0 10px 40px -10px rgba(236, 72, 153, 0.7),
    0 0 0 2px rgba(255, 255, 255, 0.1) inset;
  animation: shimmer 4s ease infinite, pulse-btn 2.2s ease-in-out infinite;
}
@keyframes shimmer {
  0%, 100% { background-position: 0% 50%; }
  50%      { background-position: 100% 50%; }
}
@keyframes pulse-btn {
  0%, 100% { box-shadow: 0 10px 40px -10px rgba(236, 72, 153, 0.7), 0 0 0 2px rgba(255,255,255,0.1) inset; }
  50%      { box-shadow: 0 10px 50px -5px rgba(168, 85, 247, 0.9), 0 0 0 2px rgba(255,255,255,0.2) inset; }
}
</style>

