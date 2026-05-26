<template>
  <div class="flex flex-col h-full overflow-hidden">

    <!-- ── Phase 1: Pick a relationship model ── -->
    <Transition name="pop" mode="out-in">
      <div v-if="phase === 'select'" key="select" class="flex flex-col h-full px-5 py-6 gap-6 overflow-hidden">

        <p class="question-text text-center shrink-0">
          Which Relationship Model would fit us best?
        </p>

        <div class="flex-1 overflow-y-auto flex flex-col gap-3 pb-2">
          <button
            v-for="(opt, i) in options"
            :key="i"
            class="option-btn"
            :class="{ selected: selected === i, rejected: rejected.includes(i) }"
            @click="selected = i"
          >
            <span class="option-check">{{ selected === i ? '✓' : '' }}</span>
            <span class="option-label" :class="{ 'line-through opacity-40': rejected.includes(i) }">{{ opt }}</span>
          </button>
        </div>

        <button
          class="shrink-0 w-full py-5 rounded-full font-bold text-xl active:scale-95 transition-all"
          :class="selected !== null
            ? 'bg-gradient-to-r from-pink-500 to-fuchsia-500 text-white shadow-lg shadow-pink-500/30'
            : 'bg-white/10 text-white/30 cursor-not-allowed'"
          :disabled="selected === null"
          @click="startYears"
        >
          Weiter →
        </button>

      </div>
    </Transition>

    <!-- ── Phase 2: Year questions 1–20 ── -->
    <Transition name="pop" mode="out-in">
      <div v-if="phase === 'years'" key="years" class="flex flex-col h-full px-5 py-6 text-center overflow-hidden">

        <Transition name="pop" mode="out-in">
          <div :key="yearStep" class="flex flex-col h-full overflow-hidden">

            <div class="flex-1 overflow-y-auto flex items-center justify-center py-4">
              <p class="question-text">
                {{ yearQuestion }}
              </p>
            </div>

            <div class="flex flex-col items-center gap-4 w-full shrink-0 pt-2">
              <button
                class="yes-btn w-full py-5 rounded-full text-white font-extrabold text-2xl active:scale-95 transition-transform"
                @click="nextYear"
              >
                Yes 💕
              </button>
              <button
                class="w-full py-5 rounded-full bg-white/10 border border-white/20 text-white font-bold text-xl active:scale-95 transition-transform"
                @click="reset"
              >
                No... ←
              </button>
            </div>

          </div>
        </Transition>

      </div>
    </Transition>

    <!-- ── Phase 3: Switch phones — confirm ── -->
    <Transition name="pop" mode="out-in">
      <div v-if="phase === 'confirm'" key="confirm" class="flex flex-col h-full px-5 py-6 items-center justify-around text-center">

        <div class="flex flex-col items-center gap-10 w-full">

          <p class="question-text">
            You as well?
          </p>

          <div class="flex flex-col items-center gap-4 w-full">
            <button
              class="yes-btn w-full py-5 rounded-full text-white font-extrabold text-2xl active:scale-95 transition-transform"
              @click="emit('answer', true)"
            >
              Yes 💕
            </button>
            <button
              class="w-full py-5 rounded-full bg-white/10 border border-white/20 text-white font-bold text-xl active:scale-95 transition-transform"
              @click="reset"
            >
              No... ←
            </button>
          </div>

        </div>

      </div>
    </Transition>

  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  slide: { type: Object, required: true },
})
const emit = defineEmits(['answer'])

const options = [
  'Something (current)',
  "Friends Who don't fuck",
  'Friends Who Fuck',
  "Friends Who fuck but don't kiss — because it would be too close to something real and would definitely cross a border, because one or the other might feel something different, ending up in an endless Loop of overwhelming emitions, coping feelings away and pretending like you don't have feeling for the other person, even though you think about this person every fucking day and can't sleep due to jealosy or unanswered questions ... but we still friends and we're drawn to each other on so many levels no? :)",
  'Mono Couple',
  'Poly Couple',
  'Married Couple (Whatever)',
  'Slave Relation (Roleplay?)',
  'Boss/Employee Relation (Roleplay?)',
]

const timeSteps = [
  { label: '1 month',   text: 'in 1 month?' },
  { label: '6 months',  text: 'in 6 months?' },
  { label: '1 year',    text: 'in 1 year?' },
]

const phase = ref('select')
const selected = ref(null)
const rejected = ref([])
const yearStep = ref(0)

const yearQuestion = computed(() => {
  const label = options[selected.value]
  const time = timeSteps[yearStep.value].text
  return `Could you see yourself with me\nas ${label}\n${time}`
})

function startYears() {
  if (selected.value === null) return
  yearStep.value = 0
  phase.value = 'years'
}

function nextYear() {
  if (yearStep.value < timeSteps.length - 1) {
    yearStep.value++
  } else {
    phase.value = 'confirm'
  }
}

function reset() {
  if (selected.value !== null && !rejected.value.includes(selected.value)) {
    rejected.value.push(selected.value)
  }
  selected.value = null
  yearStep.value = 0
  phase.value = 'select'
}
</script>

<style scoped>
.question-text {
  font-size: clamp(1.4rem, 6vw, 2.2rem);
  font-weight: 900;
  color: #fff;
  line-height: 1.25;
  white-space: pre-line;
  text-shadow: 0 0 30px rgba(236, 72, 153, 0.35);
}

.option-btn {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.9rem 1.1rem;
  border-radius: 1rem;
  background: rgba(255, 255, 255, 0.07);
  border: 1.5px solid rgba(255, 255, 255, 0.12);
  color: #fff;
  font-size: 1rem;
  font-weight: 600;
  text-align: left;
  transition: background 0.15s, border-color 0.15s;
  cursor: pointer;
}

.option-btn.selected {
  background: rgba(236, 72, 153, 0.18);
  border-color: rgba(236, 72, 153, 0.7);
}

.option-btn.rejected {
  opacity: 0.5;
  cursor: not-allowed;
  pointer-events: none;
}

.option-check {
  width: 1.4rem;
  height: 1.4rem;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  border: 2px solid rgba(255, 255, 255, 0.3);
  font-size: 0.85rem;
  color: #ec4899;
  font-weight: 900;
  background: rgba(0,0,0,0.2);
}

.option-btn.selected .option-check {
  border-color: #ec4899;
  background: rgba(236, 72, 153, 0.2);
}

.option-label {
  flex: 1;
}

.yes-btn {
  background: linear-gradient(135deg, #ec4899 0%, #a855f7 50%, #6366f1 100%);
  box-shadow: 0 0 30px rgba(168, 85, 247, 0.4);
}

/* Transition */
.pop-enter-active,
.pop-leave-active {
  transition: opacity 0.25s ease, transform 0.25s ease;
}
.pop-enter-from {
  opacity: 0;
  transform: scale(0.95) translateY(12px);
}
.pop-leave-to {
  opacity: 0;
  transform: scale(1.03);
}
</style>
