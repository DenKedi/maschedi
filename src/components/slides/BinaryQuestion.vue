<template>
  <div class="flex flex-col h-full px-5 py-6 items-center justify-around text-center">

    <!-- Rain overlay -->
    <div class="binary-rain" aria-hidden="true">
      <span v-for="i in 18" :key="i" class="rain-col" :style="`--i:${i}`">
        10010001001011011001110
      </span>
    </div>

    <div class="relative z-10 flex flex-col items-center gap-8 w-full">

      <p class="text-white/50 text-sm font-semibold tracking-widest uppercase">Did Someone Say Numbers?</p>

      <h2 class="binary-question font-extrabold leading-tight tracking-widest">
        10010001001011011001110<span class="text-pink-400">?</span>
      </h2>

      <img src="/images/piggy.gif" alt="" class="w-36 h-36 object-contain rounded-2xl" />

      <Transition name="pop" mode="out-in">

        <!-- Options -->
        <div v-if="!answered" key="options" class="flex gap-6 justify-center w-full">
          <button
            v-for="opt in ['0', '1']"
            :key="opt"
            class="binary-btn"
            @click="answered = true"
          >
            {{ opt }}
          </button>
        </div>

        <!-- Reaction -->
        <div v-else key="reaction" class="flex flex-col items-center gap-6">
          <div class="reaction-text">Das hätte ich nicht von dir gedacht...</div>
          <button
            class="mt-2 px-10 py-5 rounded-full bg-gradient-to-r from-pink-500 to-fuchsia-500 text-white font-bold text-xl active:scale-95 transition-transform shadow-lg shadow-pink-500/30"
            @click="emit('answer', true)"
          >
            Weiter →
          </button>
        </div>

      </Transition>

    </div>

  </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
  slide: { type: Object, required: true },
})
const emit = defineEmits(['answer'])

const answered = ref(false)
</script>

<style scoped>
/* Binary rain background */
.binary-rain {
  position: absolute;
  inset: 0;
  display: flex;
  justify-content: space-around;
  overflow: hidden;
  pointer-events: none;
  z-index: 0;
  opacity: 0.08;
}
.rain-col {
  font-family: monospace;
  font-size: 0.7rem;
  color: #a855f7;
  writing-mode: vertical-rl;
  animation: rain-fall calc(4s + var(--i) * 0.3s) linear infinite;
  animation-delay: calc(var(--i) * -0.5s);
  white-space: nowrap;
  letter-spacing: 0.5em;
}
@keyframes rain-fall {
  from { transform: translateY(-100%); }
  to   { transform: translateY(110vh); }
}

/* Question */
.binary-question {
  font-size: clamp(1.1rem, 5vw, 1.6rem);
  font-family: monospace;
  color: #e2e8f0;
  text-shadow: 0 0 20px rgba(168, 85, 247, 0.6);
  word-break: break-all;
  max-width: 320px;
}

/* 0 / 1 buttons */
.binary-btn {
  width: 90px;
  height: 90px;
  border-radius: 1.25rem;
  font-size: 2.5rem;
  font-weight: 900;
  font-family: monospace;
  color: #fff;
  background: rgba(168, 85, 247, 0.15);
  border: 2px solid rgba(168, 85, 247, 0.5);
  box-shadow: 0 0 24px rgba(168, 85, 247, 0.25);
  transition: transform 0.1s, box-shadow 0.2s;
}
.binary-btn:active {
  transform: scale(0.9);
}
.binary-btn:hover {
  background: rgba(168, 85, 247, 0.28);
  box-shadow: 0 0 36px rgba(168, 85, 247, 0.5);
}

/* Reaction */
.reaction-text {
  font-size: clamp(1.4rem, 6vw, 2rem);
  font-weight: 800;
  color: #fff;
  text-shadow: 0 0 20px rgba(236, 72, 153, 0.4);
  max-width: 300px;
  line-height: 1.3;
}

/* Transition */
.pop-enter-active { transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1); }
.pop-leave-active { transition: all 0.18s ease-in; }
.pop-enter-from   { opacity: 0; transform: scale(0.85); }
.pop-leave-to     { opacity: 0; transform: scale(0.85); }
</style>
