<script setup>
import { ref, watch, computed } from 'vue';
import Card from './component/Card.vue';

// 試完成以下功能：
//  1. 點擊卡片，卡片會翻開 (已完成)
//  2. 點擊兩張不同的卡片，卡片會翻回去 
//  3. 點擊兩張相同的卡片，卡片會消失
//  4. 當所有卡片都消失時，顯示「恭喜破關，再來一局？」的對話框，按下確定後重置遊戲
//  5. 將卡片獨立抽出為 Card.vue 元件

const cards = ref([]);
const openedCard = ref([]);
const openedCardCount = computed(()=> openedCard.value.length);
const tempOpenedCard = ref([]);
const tempOpenedCardIdx = ref([]);
const matchedCard = ref([]);
const isReset = ref(false);

watch(
  openedCardCount,
  (newVal) => {
    // 全部配對完成
    if (newVal === cards.value.length) {
      window.setTimeout(() => {
        isReset.value = confirm('恭喜破關，再來一局');
      }, 1000);
    }

    // 開啟數量為雙數
    if (newVal % 2  === 0) {
      const tempSet = [...new Set(tempOpenedCard.value)];
      const isTempMatched = tempOpenedCard.value.length == 2 && tempSet.length == 1;

      if (isTempMatched) {
        // 配對成功
        window.setTimeout(() => {
          matchedCard.value.push(...tempOpenedCardIdx.value);
          tempOpenedCard.value = [];
          tempOpenedCardIdx.value =[];
        }, 1600);

      } else {
        // 配對失敗
        window.setTimeout(() => {
          openedCard.value = openedCard.value.filter((e) => {
            return tempOpenedCardIdx.value.indexOf(e) === -1;
          })
          tempOpenedCard.value = [];
          tempOpenedCardIdx.value = [];
        }, 1000);
      }
    }
});

watch(
  () => isReset.value,
  (newVal) => {
    if (newVal) {
      gameInit();
    }
});

// 遊戲初始化，洗牌
const gameInit = () => {
  const numArr = [...new Array(16).keys()].map(i => ++i);
  numArr.sort(() => Math.random() - 0.5);
  cards.value = numArr.map(d => (d % 8) + 1);
  openedCard.value = [];
  matchedCard.value = [];
}

const clickHandler = (idx, n) => {

  if (tempOpenedCard.value.length < 2) {
    openedCard.value.push(idx);
    tempOpenedCard.value.push(n);
    tempOpenedCardIdx.value.push(idx);
  }

  // 點擊相同的會關閉
  if(tempOpenedCardIdx.value.length > 1 && tempOpenedCardIdx.value[0] === idx){
    window.setTimeout(() => {
      tempOpenedCard.value = [];
      tempOpenedCardIdx.value = [];
      openedCard.value = openedCard.value.filter((val)=>{
        return val !== idx
      })
    }, 100);
    return;
  }
}
</script>

<template>
  <div class="bg-emerald-900 h-[100vh] w-full top-0 left-0 z-10 absolute">

    <div class="my-10 text-white text-center ">
      <div class="mb-8 text-5xl">五倍對對碰</div>
      <button 
        @click="gameInit"
        class="rounded font-bold bg-blue-500 mx-6 text-white py-2 px-4 hover:bg-blue-700">開始</button>
    </div>

    <div class="rounded-xl mx-auto border-4 mt-12 grid grid-flow-col p-10 w-[900px] gap-2 grid-rows-4">
      
      <template v-if="cards.length > 0">
        <Card v-for="(n, idx) in cards"
          :cardNumber="n"
          :isOpen="openedCard.includes(idx)"
          :isVisible="cards[idx] > 0 && !matchedCard.includes(idx)"
          @click="clickHandler(idx, n)"
        />
      </template>

    </div>
  </div>
</template>