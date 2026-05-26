<template>
  <div class="flex flex-col h-full overflow-hidden">
    <div class="flex-1 overflow-y-auto scroll-smooth px-4 pt-5 pb-6 flex flex-col gap-10">

      <h1 class="text-4xl font-bold text-center tracking-tight">30. Juli 2025</h1>

      <template v-for="(item, i) in images" :key="i">
        <!-- Image + caption group -->
        <div class="chat-group">
          <img
            :src="`/images/chat${i + 1}.jpg`"
            :alt="`Chat ${i + 1}`"
            class="w-full rounded-2xl shadow-xl object-contain chat-img"
          />
          <div class="caption-connector"></div>
          <div class="caption-block">
            <p class="caption-text" v-html="item.caption"></p>
          </div>
        </div>

        <!-- Buttons only after last image -->
        <div v-if="i === images.length - 1" class="flex flex-col items-center gap-4 pt-2 pb-4">
          <button
            class="w-full py-5 rounded-full bg-white/10 border border-white/20 text-white font-bold text-xl active:scale-95 transition-transform"
            @click="emit('jump', 'slider')"
          >
            ← Zurück
          </button>
          <button
            class="w-full py-5 rounded-full bg-gradient-to-r from-pink-500 to-fuchsia-500 text-white font-bold text-xl active:scale-95 transition-transform shadow-lg shadow-pink-500/30"
            @click="emit('answer', true)"
          >
            Weiter →
          </button>
        </div>
      </template>

    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  slide: { type: Object, required: true },
})
const emit = defineEmits(['answer', 'jump'])

const images = [
  { caption: ' Warum sollte Alex wohl gehen 🤔' },
  { caption: 'Def... Junge sag doch nicht sowas.' },
  { caption: 'AHHHHH Dummer Junge man' },
  { caption: '... und man nichts einander versprochen hat.<br><br><em>It\'s like nothing was promised but everything felt implied.</em><br><br>Ich check sooooo jetzt' },
  { caption: 'Wo willst du hin? Ich geb dir mal zwei Optionen 👇' },
]
</script>

<style scoped>
.chat-group {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
}
.chat-img {
  margin-bottom: 0;
}
.caption-connector {
  width: 2px;
  height: 18px;
  background: linear-gradient(to bottom, rgba(236, 72, 153, 0.8), rgba(236, 72, 153, 0.2));
}
.caption-block {
  position: relative;
  background: rgba(236, 72, 153, 0.08);
  border: 1px solid rgba(236, 72, 153, 0.35);
  border-radius: 0.9rem;
  padding: 0.85rem 1.1rem;
  width: 100%;
}
.caption-block::before {
  content: '';
  position: absolute;
  top: -7px;
  left: 50%;
  transform: translateX(-50%) rotate(45deg);
  width: 12px;
  height: 12px;
  background: rgba(236, 72, 153, 0.15);
  border-top: 1px solid rgba(236, 72, 153, 0.35);
  border-left: 1px solid rgba(236, 72, 153, 0.35);
}
.caption-text {
  color: rgba(255, 255, 255, 0.9);
  font-size: 1.05rem;
  font-weight: 600;
  line-height: 1.6;
  text-align: left;
}
</style>

<style scoped>
.answer-input {
  width: 100%;
  max-width: 20rem;
  padding: 0.85rem 1.2rem;
  border-radius: 1rem;
  background: rgba(255, 255, 255, 0.07);
  border: 1.5px solid rgba(255, 255, 255, 0.18);
  color: #fff;
  font-size: 1.1rem;
  outline: none;
  transition: border-color 0.2s, box-shadow 0.2s;
}
.answer-input::placeholder {
  color: rgba(255, 255, 255, 0.3);
}
.answer-input:focus {
  border-color: rgba(236, 72, 153, 0.6);
  box-shadow: 0 0 0 3px rgba(236, 72, 153, 0.15);
}

.question-block {
  border-top: 1px solid rgba(255, 255, 255, 0.08);
  margin-top: 0.5rem;
}

button:disabled {
  opacity: 0.4;
}
</style>
