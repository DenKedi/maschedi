<template>
  <div class="flex flex-col h-full px-5 py-6 items-center justify-around text-center">

    <Transition name="pop" mode="out-in">
      <div :key="step" class="flex flex-col items-center gap-10 w-full">

        <p class="question-text">
          {{ questions[step] }}
        </p>

        <button
          class="yes-btn py-5 px-14 rounded-full text-white font-extrabold text-2xl active:scale-95 transition-transform"
          @click="next"
        >
          Yes 💕
        </button>

      </div>
    </Transition>

  </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
  slide: { type: Object, required: true },
})
const emit = defineEmits(['answer'])

const questions = [
  'Could you see yourself with me\nin 1 year?',
  'In 5?',
  'In 20?',
]

const step = ref(0)

function next() {
  if (step.value < questions.length - 1) {
    step.value++
  } else {
    emit('answer', true)
  }
}
</script>

<style scoped>
.question-text {
  font-size: clamp(1.6rem, 7vw, 2.6rem);
  font-weight: 900;
  color: #fff;
  line-height: 1.25;
  white-space: pre-line;
  text-shadow: 0 0 30px rgba(236, 72, 153, 0.35);
}

.yes-btn {
  background: linear-gradient(135deg, #ec4899 0%, #a855f7 50%, #6366f1 100%);
  background-size: 200% 200%;
  box-shadow:
    0 10px 40px -10px rgba(236, 72, 153, 0.7),
    0 0 0 2px rgba(255, 255, 255, 0.1) inset;
  animation: shimmer 4s ease infinite;
}
@keyframes shimmer {
  0%, 100% { background-position: 0% 50%; }
  50%       { background-position: 100% 50%; }
}

.pop-enter-active { transition: all 0.35s cubic-bezier(0.34, 1.56, 0.64, 1); }
.pop-leave-active { transition: all 0.18s ease-in; }
.pop-enter-from   { opacity: 0; transform: scale(0.85) translateY(20px); }
.pop-leave-to     { opacity: 0; transform: scale(0.85) translateY(-20px); }
</style>
