<template>
  <div class="fixed top-0 left-0 right-0 h-1 bg-white/10 z-50">
    <div
      class="h-full bg-pink-500 transition-all duration-500 ease-out"
      :style="{ width: `${percentage}%` }"
    />
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  current: { type: Number, required: true },
  total: { type: Number, required: true },
})

const percentage = computed(() => Math.round((props.current / props.total) * 100))
</script>
