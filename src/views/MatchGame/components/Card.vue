<template>
  <div
    class="flip-card"
    :class="{
      open: openedCard.includes(idx),
    }"
    @click="$emit('handler',{n , idx})"
  >
    <div class="flip-card-inner" v-if="cards[idx] > 0">
      <div class="flip-card-front">{{ n }}</div>
      <div class="flip-card-back">
        <img :src="`./img/cat-0${n}.jpg`" alt="" />
      </div>
    </div>
  </div>
</template>
<script setup>
import { ref,watch } from 'vue';

const props =defineProps({
  n : Number,
  idx : Number,
  openedCard: Array,
  cards : Array
})

const emit = defineEmits(['handler'])

</script>
<style scoped src="../MatchGame.css"></style>