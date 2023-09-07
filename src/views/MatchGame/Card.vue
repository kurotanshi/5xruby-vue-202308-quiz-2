<script setup>
import { toRefs } from 'vue';
const props = defineProps({
    n: {
        type: Number
    },
    isOpen: {
        type: Boolean
    },
    isCardExist: {
        type: Boolean
    }
})

const { n, isOpen, isCardExist } = toRefs(props);
const emit = defineEmits(['click']);


</script>

<template>
    <div class="flip-card" :class="{ 'open': isOpen }" @click="emit('click')">
        <div style="text-align: center;font-weight: bold;color: aliceblue;">{{ n }}</div>
        <div class="flip-card-inner" v-if="isCardExist">
            <div class="flip-card-front"></div>
            <div class="flip-card-back">
                <img :src="`./img/cat-0${n}.jpg`" alt="">
            </div>
        </div>
    </div>
</template>

<style scoped src="./MatchGame.css"></style>