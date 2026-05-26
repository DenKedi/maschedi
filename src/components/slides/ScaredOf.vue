<template>
  <div class="flex flex-col h-full px-6 py-10 select-none">

    <!-- Label -->
    <p class="label">You once asked me if I was scared</p>

    <div class="intro-row">
      <p class="intro-text">I thought about it.</p>
      <button class="icon-button" @click="expanded = true" v-if="!expanded" aria-label="Show my answer">
        →
      </button>
    </div>

    <transition name="fade">
      <div v-if="expanded" class="fear-card">
        <p class="fear-text">
          you not actually liking me for my personality but for the feelings I give you.
        </p>
      </div>
    </transition>

    <div v-if="expanded" class="divider" />

    <div v-if="expanded">
      <p class="and-you">And You?</p>

      <textarea
        v-model="text"
        placeholder="What Scares You..."
        class="flex-1 w-full rounded-2xl bg-white/5 border-2 border-white/10 text-white text-lg p-5 resize-none focus:outline-none focus:border-pink-400 placeholder:text-white/25 transition-colors"
      />

      <button
        :disabled="!text.trim()"
        :class="[
          'mt-6 w-full py-5 rounded-2xl font-bold text-xl transition-all duration-200',
          text.trim()
            ? 'bg-gradient-to-r from-pink-500 to-fuchsia-500 text-white active:scale-95 shadow-lg shadow-pink-500/30'
            : 'bg-white/5 text-white/20 cursor-not-allowed',
        ]"
        @click="text.trim() && emit('answer', text.trim())"
      >
        Next →
      </button>
    </div>

  </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
  slide: { type: Object, required: true },
})
const emit = defineEmits(['answer'])

const text = ref('')
const expanded = ref(false)
</script>

<style scoped>
.label {
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: rgba(236, 72, 153, 0.7);
  margin-bottom: 1.1rem;
}

.fear-card {
  background: rgba(236, 72, 153, 0.07);
  border: 1px solid rgba(236, 72, 153, 0.25);
  border-radius: 1.25rem;
  padding: 1.4rem 1.3rem;
}

.fear-text {
  font-size: clamp(1.15rem, 4.5vw, 1.5rem);
  font-weight: 700;
  color: rgba(255, 255, 255, 0.92);
  line-height: 1.55;
  font-style: italic;
}

.intro-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  margin-bottom: 1.4rem;
}

.intro-text {
  font-size: 1rem;
  color: rgba(255, 255, 255, 0.85);
  line-height: 1.8;
  font-weight: 600;
}

.icon-button {
  width: 3rem;
  height: 3rem;
  border-radius: 9999px;
  display: grid;
  place-items: center;
  background: rgba(255, 255, 255, 0.12);
  color: #fff;
  border: 1px solid rgba(255, 255, 255, 0.18);
  font-size: 1.3rem;
  cursor: pointer;
  transition: transform 200ms ease, background 200ms ease, border-color 200ms ease;
}

.icon-button:hover {
  transform: translateY(-1px);
  background: rgba(255, 255, 255, 0.18);
  border-color: rgba(236, 72, 153, 0.4);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.divider {
  width: 2px;
  height: 2rem;
  background: linear-gradient(to bottom, rgba(236, 72, 153, 0.5), transparent);
  align-self: center;
  margin: 0.8rem 0;
}

.and-you {
  font-size: 1.6rem;
  font-weight: 900;
  color: #fff;
  margin-bottom: 1rem;
  text-shadow: 0 0 30px rgba(236, 72, 153, 0.4);
}
</style>
