<template>
  <div class="relative flex flex-col h-full select-none">

    <!-- Background image or gradient placeholder -->
    <div class="absolute inset-0">
      <img
        v-if="slide.image"
        :src="slide.image"
        alt=""
        class="w-full h-full object-cover"
      />
      <div
        v-else
        class="w-full h-full bg-gradient-to-br from-pink-900/60 via-purple-900/60 to-black"
      />
    </div>

    <!-- Gradient overlay for text legibility -->
    <div class="absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent" />

    <!-- Content -->
    <div class="relative flex flex-col justify-end h-full px-6 pb-12">
      <h1 class="text-4xl font-extrabold text-white leading-tight mb-3">
        {{ slide.title }}
      </h1>
      <p v-if="slide.subtitle" class="text-white/70 text-lg mb-8">
        {{ slide.subtitle }}
      </p>
      <button
        class="w-full py-5 rounded-2xl bg-pink-500 text-white font-bold text-xl active:scale-95 transition-transform"
        @click="emit('answer', true)"
      >
        {{ slide.buttonText ?? 'Weiter →' }}
      </button>
    </div>

  </div>
</template>

<script setup>
const props = defineProps({
  slide: { type: Object, required: true },
})
const emit = defineEmits(['answer'])
</script>
