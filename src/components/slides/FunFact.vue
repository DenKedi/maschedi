<template>
  <div class="relative flex flex-col h-full px-5 py-6 items-center justify-around text-center overflow-y-auto">

    <!-- Sekt decoration -->
    <img src="/images/sekt.jpg" class="sekt-img" alt="" />

    <div class="flex flex-col items-center gap-5 w-full max-w-sm">

      <!-- Tag -->
      <span class="tag">💡 Eine Menge Spaß 💡</span>
      <span class="tag">Mit Daten aus unserem Chat </span>

      <!-- Stats row -->
      <div class="stats-grid">
        <div class="stat-card">
          <span class="stat-value">03:30</span>
          <span class="stat-label">Minuten Scrollzeit</span>
        </div>
        <div class="stat-card">
          <span class="stat-value">555</span>
          <span class="stat-label">Bilder &amp; Videos</span>
        </div>
        <div class="stat-card stat-wide">
          <span class="stat-value">469 MB</span>
          <span class="stat-label">Daten</span>
        </div>
      </div>

      <!-- Date range -->
      <p class="date-range">29. Juni 2025 — 26. Mai 2026</p>

      <!-- Body text -->
      <div class="body-text">
        <p>
          Ich glaube mit keinem anderen Menschen habe ich im letzten Jahr so viel geteilt wie mit dir. <span class="highlight">Danke dass du zuhörst, liest, und einfach für mich da bist!</span>
          Warum hattest du vorher Selbstlöschende Nachrichten an!!!
        </p>
      </div>

      <button
        class="w-full max-w-xs py-5 rounded-full bg-gradient-to-r from-pink-500 to-fuchsia-500 text-white font-bold text-xl active:scale-95 transition-transform shadow-lg shadow-pink-500/30 mt-2"
        @click="emit('answer', true)"
      >
        Weiter →
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

<style scoped>
.tag {
  display: inline-block;
  padding: 0.3rem 1rem;
  border-radius: 9999px;
  background: rgba(168, 85, 247, 0.2);
  border: 1px solid rgba(168, 85, 247, 0.5);
  color: #d8b4fe;
  font-size: 0.85rem;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.stats-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.75rem;
  width: 100%;
}

.stat-card {
  background: rgba(255, 255, 255, 0.06);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 1.25rem;
  padding: 1rem 0.75rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.25rem;
}

.stat-wide {
  grid-column: span 2;
}

.stat-value {
  font-size: clamp(1.6rem, 7vw, 2.2rem);
  font-weight: 900;
  background: linear-gradient(135deg, #ec4899, #a855f7);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  line-height: 1;
}

.stat-label {
  font-size: 0.75rem;
  color: rgba(255, 255, 255, 0.5);
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.06em;
}

.date-range {
  font-size: 0.85rem;
  color: rgba(255, 255, 255, 0.4);
  font-weight: 600;
  letter-spacing: 0.04em;
}

.body-text {
  font-size: 0.95rem;
  color: rgba(255, 255, 255, 0.75);
  line-height: 1.65;
  text-align: left;
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 1.25rem;
  padding: 1.1rem 1.2rem;
}

.body-text .highlight {
  background: linear-gradient(135deg, rgba(236, 72, 153, 0.35), rgba(168, 85, 247, 0.35));
  border-radius: 4px;
  padding: 0.05em 0.2em;
  color: #fff;
  font-weight: 600;
}

.sekt-img {
  position: absolute;
  left: 8%;
  top: 46%;
  transform: translateY(-50%);
  width: clamp(70px, 18vw, 110px);
  height: auto;
  border-radius: 1rem;
  object-fit: cover;
  pointer-events: none;
  opacity: 0.9;
}
</style>
