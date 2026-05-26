<template>
  <div class="flex flex-col h-full px-6 py-10 select-none overflow-hidden">
    <h2 class="text-2xl font-extrabold text-white text-center mb-6 shrink-0">
      {{ slide.title }}
    </h2>

    <!-- Scrollable image stack -->
    <div class="flex-1 overflow-y-auto space-y-4 pb-4">
      <div
        v-for="(img, i) in slide.images"
        :key="i"
        class="rounded-2xl overflow-hidden"
      >
        <img
          :src="img"
          :alt="`Bild ${i + 1}`"
          class="w-full object-cover"
        />
      </div>
      <div
        v-if="!slide.images || slide.images.length === 0"
        class="rounded-2xl bg-white/5 border-2 border-dashed border-white/20 flex items-center justify-center h-48 text-white/30 text-sm"
      >
        Noch keine Bilder
      </div>
    </div>

    <button
      class="mt-6 shrink-0 w-full py-5 rounded-2xl bg-pink-500 text-white font-bold text-xl active:scale-95 transition-transform"
      @click="emit('answer', true)"
    >
      Weiter →
    </button>
  </div>
</template>

<script setup>
const props = defineProps({
  slide: { type: Object, required: true },
})
const emit = defineEmits(['answer'])
</script>
