<template>
  <div class="flex flex-col h-full px-6 py-10 select-none">

    <!-- Phase: p1 — Player 1 picks -->
    <template v-if="phase === 'p1'">
      <p class="text-pink-400 text-sm font-semibold uppercase tracking-widest text-center mb-2">
        {{ slide.player1 }} ist dran
      </p>
      <p class="text-2xl font-bold text-white text-center leading-snug mb-10">
        {{ slide.question }}
      </p>
      <div class="grid grid-cols-2 gap-4 flex-1 content-center">
        <button
          v-for="(option, i) in slide.options"
          :key="i"
          :class="[
            'rounded-2xl px-4 py-6 text-lg font-semibold transition-all duration-200 border-2',
            p1Answer === i
              ? 'bg-pink-500 border-pink-500 text-white scale-105'
              : 'bg-white/5 border-white/10 text-white hover:bg-white/10 active:scale-95',
          ]"
          @click="p1Answer = i"
        >
          {{ option }}
        </button>
      </div>
      <button
        v-if="p1Answer !== null"
        class="mt-8 w-full py-4 rounded-2xl bg-pink-500 text-white font-bold text-xl active:scale-95 transition-transform"
        @click="phase = 'handoff'"
      >
        Fertig →
      </button>
    </template>

    <!-- Phase: handoff — hide P1 answer, pass to P2 -->
    <template v-else-if="phase === 'handoff'">
      <div class="flex flex-col items-center justify-center h-full gap-8 text-center">
        <div class="text-6xl">🙈</div>
        <p class="text-3xl font-bold text-white">
          {{ slide.player1 }} hat geantwortet!
        </p>
        <p class="text-white/50 text-lg">
          Gib das Handy an <span class="text-pink-400 font-semibold">{{ slide.player2 }}</span> weiter.
        </p>
        <button
          class="mt-4 px-10 py-4 rounded-2xl bg-pink-500 text-white font-bold text-xl active:scale-95 transition-transform"
          @click="phase = 'p2'"
        >
          Ich bin bereit ✓
        </button>
      </div>
    </template>

    <!-- Phase: p2 — Player 2 picks -->
    <template v-else-if="phase === 'p2'">
      <p class="text-pink-400 text-sm font-semibold uppercase tracking-widest text-center mb-2">
        {{ slide.player2 }} ist dran
      </p>
      <p class="text-2xl font-bold text-white text-center leading-snug mb-10">
        {{ slide.question }}
      </p>
      <div class="grid grid-cols-2 gap-4 flex-1 content-center">
        <button
          v-for="(option, i) in slide.options"
          :key="i"
          :class="[
            'rounded-2xl px-4 py-6 text-lg font-semibold transition-all duration-200 border-2',
            p2Answer === i
              ? 'bg-purple-500 border-purple-500 text-white scale-105'
              : 'bg-white/5 border-white/10 text-white hover:bg-white/10 active:scale-95',
          ]"
          @click="p2Answer = i"
        >
          {{ option }}
        </button>
      </div>
      <button
        v-if="p2Answer !== null"
        class="mt-8 w-full py-4 rounded-2xl bg-purple-500 text-white font-bold text-xl active:scale-95 transition-transform"
        @click="phase = 'reveal'"
      >
        Auflösung →
      </button>
    </template>

    <!-- Phase: reveal — show both answers side by side -->
    <template v-else-if="phase === 'reveal'">
      <p class="text-2xl font-bold text-white text-center leading-snug mb-8">
        Auflösung!
      </p>
      <div class="flex gap-4 flex-1 content-center">
        <div class="flex-1 rounded-2xl bg-pink-500/20 border-2 border-pink-500 p-6 flex flex-col items-center justify-center gap-3 text-center">
          <p class="text-pink-400 text-xs font-bold uppercase tracking-widest">{{ slide.player1 }}</p>
          <p class="text-white text-xl font-bold">{{ slide.options[p1Answer] }}</p>
        </div>
        <div class="flex-1 rounded-2xl bg-purple-500/20 border-2 border-purple-500 p-6 flex flex-col items-center justify-center gap-3 text-center">
          <p class="text-purple-400 text-xs font-bold uppercase tracking-widest">{{ slide.player2 }}</p>
          <p class="text-white text-xl font-bold">{{ slide.options[p2Answer] }}</p>
        </div>
      </div>
      <p v-if="p1Answer === p2Answer" class="mt-6 text-center text-green-400 text-lg font-semibold">
        Gleiche Antwort! 🎉
      </p>
      <p v-else class="mt-6 text-center text-white/50 text-lg">
        Unterschiedliche Antworten 😄
      </p>
      <button
        class="mt-6 w-full py-4 rounded-2xl bg-white/10 border border-white/20 text-white font-bold text-xl active:scale-95 transition-transform"
        @click="emit('answer', { player1: slide.options[p1Answer], player2: slide.options[p2Answer] })"
      >
        Weiter →
      </button>
    </template>

  </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
  slide: { type: Object, required: true },
})
const emit = defineEmits(['answer'])

const phase = ref('p1')
const p1Answer = ref(null)
const p2Answer = ref(null)
</script>
