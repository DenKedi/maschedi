<template>
  <div class="flex flex-col h-full px-6 py-10 select-none">
    <p class="text-2xl font-bold text-white text-center leading-snug mb-10">
      {{ slide.question }}
    </p>

    <textarea
      v-model="text"
      :placeholder="slide.placeholder || 'Schreib hier...'"
      class="flex-1 w-full rounded-2xl bg-white/5 border-2 border-white/10 text-white text-lg p-5 resize-none focus:outline-none focus:border-pink-500 placeholder:text-white/25 transition-colors"
    />

    <button
      :disabled="!text.trim()"
      :class="[
        'mt-6 w-full py-4 rounded-2xl font-bold text-xl transition-all duration-200',
        text.trim()
          ? 'bg-pink-500 text-white active:scale-95'
          : 'bg-white/5 text-white/20 cursor-not-allowed',
      ]"
      @click="text.trim() && emit('answer', text.trim())"
    >
      Absenden →
    </button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
  slide: { type: Object, required: true },
})
const emit = defineEmits(['answer'])

const text = ref('')
</script>
