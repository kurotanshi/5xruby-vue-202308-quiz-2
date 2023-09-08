<script setup>

const props = defineProps({
  cardNumber:{
    type: [String, Number]
  },
  isOpen: {
    type:Boolean
  },
  isVisible: {
    type:Boolean
  }
});

const emit = defineEmits(['click']);

</script>

<template>
  <div 
    class="flip-card" 
    :class="{ 'open': props.isOpen }" 
    @click="emit('click')"
  >
    <div class="flip-card-inner" v-if="props.isVisible">
      <div class="flip-card-front"></div>
      <div class="flip-card-back">
        <img :src="`./img/cat-0${props.cardNumber}.jpg`" alt="">
      </div>
    </div>
  </div>
</template>

<style scoped src="../MatchGame.css"></style>