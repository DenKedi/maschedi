<template>
  <div class="flex flex-col h-full px-6 py-10 select-none">
    <p class="text-2xl font-bold text-white text-center leading-snug mb-10">
      {{ slide.question }}
    </p>

    <!-- Min/Max labels -->
    <div class="flex justify-between text-white/40 text-sm mb-4 px-1">
      <span>{{ slide.minLabel }}</span>
      <span>{{ slide.maxLabel }}</span>
    </div>

    <!-- Number grid -->
    <div class="flex flex-wrap justify-center gap-3 flex-1 content-center">
      <button
        v-for="n in range"
        :key="n"
        :class="[
          'w-14 h-14 rounded-2xl text-xl font-bold transition-all duration-200 border-2',
          selected === n
            ? 'bg-pink-500 border-pink-500 text-white scale-110'
            : 'bg-white/5 border-white/10 text-white hover:bg-white/10 active:scale-95',
        ]"
        @click="selected = n"
      >
        {{ n }}
      </button>
    </div>

    <button
      v-if="selected !== null"
      class="mt-8 w-full py-5 rounded-2xl bg-pink-500 text-white font-bold text-xl active:scale-95 transition-transform"
      @click="emit('answer', selected)"
    >
      {{ selected }} — Weiter →
    </button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  slide: { type: Object, required: true },
})
const emit = defineEmits(['answer'])

const selected = ref(null)

const range = computed(() => {
  const result = []
  for (let i = props.slide.min; i <= props.slide.max; i++) result.push(i)
  return result
})
</script>
