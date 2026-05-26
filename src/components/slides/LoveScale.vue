<template>
  <div class="flex flex-col h-full px-5 py-6 items-center justify-around text-center">

    <p class="text-2xl sm:text-3xl font-extrabold text-white leading-snug title-glow">
      {{ slide.question }}
    </p>

    <!-- Container: buttons or richtig screen -->
    <div class="relative flex items-center justify-center w-full">

      <!-- Buttons -->
      <Transition name="pop">
        <div v-if="picked !== 10" class="grid grid-cols-5 gap-3 w-full max-w-sm">
          <template v-for="n in 10" :key="n">
            <button
              v-if="!removed.has(n)"
              class="love-btn aspect-square rounded-2xl font-extrabold text-2xl active:scale-90 transition-transform"
              :class="n === 10 ? 'btn-ten' : 'btn-regular'"
              @click="pick(n)"
            >
              {{ n }}
            </button>
            <div v-else class="aspect-square" aria-hidden="true"></div>
          </template>
        </div>
      </Transition>

      <!-- AUTSCH flies up overlay -->
      <div
        v-if="autschKey"
        :key="autschKey"
        class="autsch-text pointer-events-none"
        :style="{ fontSize: autschSize }"
        @animationend="autschKey = 0"
      >
        AUTSCH
      </div>

      <!-- RICHTIG -->
      <Transition name="pop">
        <div v-if="picked === 10" class="flex flex-col items-center gap-5">
          <div class="richtig-emoji"></div>
          <div class="richtig-text">That's correct!!</div>
          <p class="text-white/70 text-lg">I don't think numbers apply to emotions though.</p>
          <button
            class="mt-2 px-10 py-5 rounded-full bg-gradient-to-r from-pink-500 to-fuchsia-500 text-white font-bold text-xl active:scale-95 transition-transform shadow-lg shadow-pink-500/30"
            @click="emit('answer', 10)"
          >
            Weiter →
          </button>
        </div>
      </Transition>

    </div>

  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  slide: { type: Object, required: true },
})
const emit = defineEmits(['answer'])

const picked = ref(null)
const removed = ref(new Set())
const autschKey = ref(0)
const autschValue = ref(1)

// n=1 → 6rem (biggest), n=9 → 1.5rem (smallest), linear
const autschSize = computed(() => {
  const size = 6 - (autschValue.value - 1) * (4.5 / 8)
  return `${size.toFixed(2)}rem`
})

function pick(n) {
  if (n === 10) {
    picked.value = 10
  } else {
    autschValue.value = n
    removed.value.add(n)
    removed.value = new Set(removed.value)
    autschKey.value++
  }
}
</script>

<style scoped>
.title-glow {
  text-shadow: 0 0 25px rgba(236, 72, 153, 0.4);
}

.love-btn {
  display: flex;
  align-items: center;
  justify-content: center;
}

.btn-regular {
  background: rgba(255, 255, 255, 0.08);
  border: 2px solid rgba(255, 255, 255, 0.15);
  color: #fff;
}
.btn-regular:hover {
  background: rgba(255, 255, 255, 0.14);
}

.btn-ten {
  background: linear-gradient(135deg, #ec4899, #a855f7);
  border: 2px solid rgba(255, 255, 255, 0.3);
  color: #fff;
  box-shadow: 0 6px 24px -6px rgba(236, 72, 153, 0.7);
  animation: pulse-btn 2s ease-in-out infinite;
}

@keyframes pulse-btn {
  0%, 100% { box-shadow: 0 6px 24px -6px rgba(236, 72, 153, 0.7); }
  50%       { box-shadow: 0 6px 32px -4px rgba(168, 85, 247, 0.9); }
}

/* AUTSCH – flies up, spins, fades */
.autsch-text {
  position: absolute;
  font-weight: 900;
  background: linear-gradient(135deg, #ef4444, #f97316);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  filter: drop-shadow(0 0 20px rgba(239, 68, 68, 0.6));
  animation: autsch-fly 1s cubic-bezier(0.2, 0.8, 0.4, 1) forwards;
  transform-origin: center bottom;
}

@keyframes autsch-fly {
  0%   { transform: translateY(0)    rotate(0deg)   scale(1);    opacity: 1; }
  30%  { transform: translateY(-60px) rotate(-15deg) scale(1.15); opacity: 1; }
  60%  { transform: translateY(-160px) rotate(25deg) scale(0.9);  opacity: 0.7; }
  100% { transform: translateY(-320px) rotate(-40deg) scale(0.5); opacity: 0; }
}

/* RICHTIG */
.richtig-emoji {
  font-size: 4rem;
  animation: bounce 0.5s ease-out;
}
@keyframes bounce {
  0%   { transform: scale(0.3); opacity: 0; }
  70%  { transform: scale(1.2); }
  100% { transform: scale(1); opacity: 1; }
}

.richtig-text {
  font-size: clamp(2rem, 10vw, 3.5rem);
  font-weight: 900;
  background: linear-gradient(135deg, #ec4899, #a855f7, #6366f1);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  filter: drop-shadow(0 0 20px rgba(236, 72, 153, 0.5));
}

/* Transition */
.pop-enter-active { transition: all 0.3s cubic-bezier(0.34, 1.56, 0.64, 1); }
.pop-leave-active { transition: all 0.18s ease-in; }
.pop-enter-from   { opacity: 0; transform: scale(0.85); }
.pop-leave-to     { opacity: 0; transform: scale(0.85); }
</style>
