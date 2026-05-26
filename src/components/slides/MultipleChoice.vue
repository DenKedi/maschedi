<template>
  <div class="flex flex-col h-full px-6 py-10 select-none">
    <p class="text-2xl font-bold text-white text-center leading-snug" :class="slide.image ? 'mb-4' : 'mb-10'">
      {{ slide.question }}
    </p>

    <div v-if="slide.image" class="flex justify-center mb-4">
      <img :src="slide.image" class="slide-img" alt="" />
    </div>

    <div class="grid grid-cols-2 gap-4 flex-1 content-center">
      <button
        v-for="(option, i) in slide.options"
        :key="i"
        :class="[
          'rounded-2xl px-4 py-6 text-lg font-semibold transition-all duration-200 border-2',
          selected === i
            ? 'bg-pink-500 border-pink-500 text-white scale-105'
            : 'bg-white/5 border-white/10 text-white hover:bg-white/10 active:scale-95',
        ]"
        @click="pick(i)"
      >
        {{ option }}
      </button>
    </div>

    <button
      v-if="selected !== null"
      class="mt-8 w-full py-5 rounded-2xl bg-pink-500 text-white font-bold text-xl active:scale-95 transition-transform"
      @click="emit('answer', slide.options[selected])"
    >
      Weiter →
    </button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
  slide: { type: Object, required: true },
})
const emit = defineEmits(['answer'])

const selected = ref(null)

function pick(i) {
  selected.value = i
}
</script>

<style scoped>
.slide-img {
  width: clamp(120px, 40vw, 220px);
  height: auto;
  border-radius: 1.25rem;
  object-fit: cover;
  box-shadow: 0 8px 32px -8px rgba(236, 72, 153, 0.4);
}
</style>
