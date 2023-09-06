<script setup>

import { ref, watch } from 'vue';

const props = defineProps({
    source: Array,    
});

const cards = ref(props.source);
const openedCard = ref([]);
const isMatchedCards = ref(0);

watch(() => props.source, val => {
    cards.value = val;
})

const emit = defineEmits(['click', 'resetGame']);
//const emit = defineEmits(['click']);

const onClick = function (event) {
    emit('click', event);

}

const clickHandler = (n, idx) => {
    openedCard.value.push(idx); //將目前開啟的卡片的index 存入
    //console.log(n);


    // 一秒後將 openedCard 清空 (牌面覆蓋回去)
    window.setTimeout(() => {
        if (openedCard.value.length == 2) {
            if (cards.value[openedCard.value[0]] === cards.value[openedCard.value[1]]) {
                console.log('找到了');
                //cards.value.find(cards.value[openedCard.value[0]])
                cards.value[openedCard.value[0]] = -1;
                isMatchedCards.value++;
                cards.value[openedCard.value[1]] = -1;
                isMatchedCards.value++;
                if (isMatchedCards.value === cards.value.length) {
                    let result = confirm('恭喜破關，再來一局？');
                    //if (result) gameInit();
                    if (result) reset();
                }
            }

        }
        openedCard.value = [];
    }, 2000);
}

const reset = (isReset) => {
    openedCard.value = [];
    isMatchedCards.value = 0;
    //console.log('reset');
    
    emit('resetGame', isReset);

    //props.resetGame.value();
}

</script>

<template>
    <div class="bg-emerald-900 h-[100vh] w-full top-0 left-0 z-10 absolute">

        <div class="my-10 text-white text-center ">
            <div class="mb-8 text-5xl">五倍對對碰</div>
            <!-- <button @click="gameInit"
                class="rounded font-bold bg-blue-500 mx-6 text-white py-2 px-4 hover:bg-blue-700">開始</button> -->
            <button @click="onClick($event)"
                class="rounded font-bold bg-blue-500 mx-6 text-white py-2 px-4 hover:bg-blue-700">開始</button>
            <button @click="reset(true)"
                class="rounded font-bold bg-blue-500 mx-6 text-white py-2 px-4 hover:bg-blue-700">測試</button>
        </div>

        <div class="rounded-xl mx-auto border-4 mt-12 grid grid-flow-col p-10 w-[900px] gap-2 grid-rows-4">

            <div v-for="(n, idx) in cards" class="flip-card" :class="{
                'open': openedCard.includes(idx)
            }" @click="clickHandler(n, idx)">
                <div class="flip-card-inner" v-if="cards[idx] > 0">
                    {{ idx + ',' + cards[idx] }}
                    <div class="flip-card-front"></div>
                    <div class="flip-card-back">
                        <img :src="`./img/cat-0${n}.jpg`" alt="">
                    </div>
                </div>
            </div>

            <!-- <div v-for="(n, idx) in cards" class="flip-card" :class="{
                'open': openedCard.includes(idx)
            }" @click="clickHandler(n, idx)">
                <div class="flip-card-inner" v-if="cards[idx] > 0">
                    {{ idx + ',' + cards[idx] }}
                    <div class="flip-card-front"></div>
                    <div class="flip-card-back">
                        <img :src="`./img/cat-0${n}.jpg`" alt="">
                    </div>
                </div>
            </div> -->

        </div>
    </div>
</template>

<style scoped src="./MatchGame.css"></style>