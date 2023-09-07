<script setup>
import { ref, watch, computed } from 'vue';
import card from './Card.vue'

// 試完成以下功能：
//  1. 點擊卡片，卡片會翻開 (已完成)
//  2. 點擊兩張不同的卡片，卡片會翻回去 
//  3. 點擊兩張相同的卡片，卡片會消失
//  4. 當所有卡片都消失時，顯示「恭喜破關，再來一局？」的對話框，按下確定後重置遊戲
//  5. 將卡片獨立抽出為 Card.vue 元件

const isStart = ref(true);
const cards = ref([]);
const openedCard = ref([]);
const cardsNumTotal = computed(() => {
  return cards.value.reduce(function (acc, i) {
    return acc + i;
  }, 0);
})

watch(cardsNumTotal, () => {
  if (cardsNumTotal.value === 0) {
    window.setTimeout(() => {
      let answer = window.confirm('恭喜破關，再來一局？');
      if (answer) {
        gameInit();
      }
    }, 1500)
  }
})

watch(openedCard, () => {
  const temp = [];
  if (openedCard.value.length === 2) {
    openedCard.value.forEach(element => {
      temp.push(cards.value[element])
    });

    if (temp[0] === temp[1]) {
      window.setTimeout(() => {
        openedCard.value.forEach(element => {
          cards.value[element] = null;
        });
        openedCard.value = [];
      }, 1500)
    } else {
      window.setTimeout(() => {
        openedCard.value = [];
      }, 1000);
    }

  }
}, { deep: true })

// 遊戲初始化，洗牌
const gameInit = () => {
  const numArr = [...new Array(16).keys()].map(i => ++i);
  numArr.sort(() => Math.random() - 0.5);
  cards.value = numArr.map(d => (d % 8) + 1);
  openedCard.value = [];
  isStart.value = false;
}

const clickHandler = (idx) => {
  if (openedCard.value.length < 2 && !openedCard.value.includes(idx)) {
    openedCard.value.push(idx);
  }
}


</script>



<template>
  <div class="bg-emerald-900 h-[100vh] w-full top-0 left-0 z-10 absolute">
    <div class="my-10 text-white text-center ">
      <div class="mb-8 text-5xl">五倍對對碰</div>
      <button @click="gameInit" class="rounded font-bold bg-blue-500 mx-6 text-white py-2 px-4 hover:bg-blue-700">
        {{ isStart ? '開始' : '重置' }}
      </button>
    </div>

    <div class="rounded-xl mx-auto border-4 mt-12 grid grid-flow-col p-10 w-[900px] gap-2 grid-rows-4">

      <!-- <div v-for="(n, idx) in cards" class="flip-card" :class="{
        'open': openedCard.includes(idx)
      }" @click="clickHandler(idx)">
        <div style="text-align: center;font-weight: bold;color: aliceblue;">{{ n }}</div>
        <div class="flip-card-inner" v-if="cards[idx] > 0">
          <div class="flip-card-front"></div>
          <div class="flip-card-back">
            <img :src="`./img/cat-0${n}.jpg`" alt="">
          </div>
        </div>
      </div> -->

      <card v-for="(n, idx) in cards" @click="clickHandler(idx)" :isOpen="openedCard.includes(idx)"
        :isCardExist="cards[idx] > 0" :n="n" />
    </div>
  </div>
</template>

<!-- <style scoped src="./MatchGame.css"></style> -->