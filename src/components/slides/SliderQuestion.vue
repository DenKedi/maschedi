<template>
  <div class="flex flex-col h-full px-5 py-6 select-none gap-4">
    <p class="text-xl sm:text-2xl font-bold text-white text-center leading-snug">
      {{ slide.question }}
    </p>

    <div v-if="slide.image" class="flex justify-center">
      <img
        :src="slide.image"
        :alt="slide.question"
        class="max-h-48 sm:max-h-56 w-auto object-contain rounded-2xl shadow-lg shadow-pink-500/20"
      />
    </div>

    <!-- Big value display -->
    <div class="flex-1 flex items-center justify-center min-h-0">
      <span
        class="font-extrabold text-pink-400 tabular-nums text-center leading-none drop-shadow-[0_0_20px_rgba(236,72,153,0.4)]"
        :class="valueSizeClass"
      >
        {{ formattedValue }}<span class="text-2xl text-white/40">{{ slide.unit ?? '' }}</span>
      </span>
    </div>

    <!-- Slider track -->
    <div class="px-1">
      <input
        v-model.number="value"
        type="range"
        :min="slide.min"
        :max="slide.max"
        :step="slide.step ?? 1"
        class="w-full h-3 appearance-none rounded-full cursor-pointer accent-pink-500"
        :style="{
          background: `linear-gradient(to right, #ec4899 0%, #ec4899 ${progress}%, rgba(255,255,255,0.12) ${progress}%, rgba(255,255,255,0.12) 100%)`,
        }"
      />
      <div class="flex justify-between text-white/40 text-xs mt-2 tabular-nums">
        <span>{{ formatNumber(slide.min) }}{{ slide.unit ?? '' }}</span>
        <span>{{ formatNumber(slide.max) }}{{ slide.unit ?? '' }}</span>
      </div>
    </div>

    <button
      class="w-full py-5 rounded-2xl bg-gradient-to-r from-pink-500 to-fuchsia-500 text-white font-bold text-xl active:scale-95 transition-transform shadow-lg shadow-pink-500/30"
      @click="emit('answer', value)"
    >
      Bestätigen →
    </button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  slide: { type: Object, required: true },
})
const emit = defineEmits(['answer'])

const value = ref(
  Math.round((props.slide.min + props.slide.max) / 2)
)

function formatNumber(n) {
  return n.toLocaleString('de-DE')
}

const formattedValue = computed(() => formatNumber(value.value))

const progress = computed(
  () => ((value.value - props.slide.min) / (props.slide.max - props.slide.min)) * 100
)

const valueSizeClass = computed(() => {
  const len = formattedValue.value.length
  if (len > 8) return 'text-5xl'
  if (len > 6) return 'text-6xl'
  if (len > 4) return 'text-7xl'
  return 'text-8xl'
})
</script>
