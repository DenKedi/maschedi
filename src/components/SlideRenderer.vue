<template>
  <component
    :is="slideComponent"
    v-if="slideComponent"
    :slide="slide"
    @answer="(val) => emit('answer', val)"
    @jump="(type) => emit('jump', type)"
  />
  <div v-else class="flex items-center justify-center h-full text-white/40 text-lg">
    Unknown slide type: {{ slide.type }}
  </div>
</template>

<script setup>
import { computed } from 'vue'
import MultipleChoice from './slides/MultipleChoice.vue'
import MultipleChoiceTwoPlayer from './slides/MultipleChoiceTwoPlayer.vue'
import ImageTitle from './slides/ImageTitle.vue'
import MultiImageTitle from './slides/MultiImageTitle.vue'
import ScaleQuestion from './slides/ScaleQuestion.vue'
import FreeText from './slides/FreeText.vue'
import SliderQuestion from './slides/SliderQuestion.vue'
import LoveScale from './slides/LoveScale.vue'
import BinaryQuestion from './slides/BinaryQuestion.vue'
import FunFact from './slides/FunFact.vue'
import ChatGallery from './slides/ChatGallery.vue'
import YesChain from './slides/YesChain.vue'
import ScaredOf from './slides/ScaredOf.vue'
import RelationshipChain from './slides/RelationshipChain.vue'

const props = defineProps({
  slide: { type: Object, required: true },
})
const emit = defineEmits(['answer', 'jump'])

const typeMap = {
  'multiple-choice': MultipleChoice,
  'multiple-choice-two-player': MultipleChoiceTwoPlayer,
  'image-title': ImageTitle,
  'multi-image-title': MultiImageTitle,
  'scale': ScaleQuestion,
  'free-text': FreeText,
  'slider': SliderQuestion,
  'love-scale': LoveScale,
  'binary': BinaryQuestion,
  'funfact': FunFact,
  'chat-gallery': ChatGallery,
  'yes-chain': YesChain,
  'scared-of': ScaredOf,
  'relationship-chain': RelationshipChain,
}

const slideComponent = computed(() => typeMap[props.slide.type] ?? null)
</script>
