<script setup>
import { ref, watch } from 'vue';

// 試完成以下功能：
//  1. 點擊卡片，卡片會翻開 (已完成)
//  2. 點擊兩張不同的卡片，卡片會翻回去 
//  3. 點擊兩張相同的卡片，卡片會消失
//  4. 當所有卡片都消失時，顯示「恭喜破關，再來一局？」的對話框，按下確定後重置遊戲
//  5. 將卡片獨立抽出為 Card.vue 元件

const cards = ref([]);
const openedCard = ref([]);

let try_count = 0;   // 嘗試次數
const opened_his = ref([]);

// 遊戲初始化，洗牌
const gameInit = () => {
  const numArr = [...new Array(16).keys()].map(i => ++i);
  numArr.sort(() => Math.random() - 0.5);
  cards.value = numArr.map(d => (d % 8) + 1);
  openedCard.value = [];
}

const clickHandler = (idx) => {    

  try_count++;
  openedCard.value.push(idx);
  opened_his.value.push(idx);

  if (openedCard.value.length >= 16) {

    window.setTimeout(() => {
      alert('恭喜破關，再來一局？');

      const numArr = [...new Array(16).keys()].map(i => ++i);
      numArr.sort(() => Math.random() - 0.5);
      cards.value = numArr.map(d => (d % 8) + 1);
      openedCard.value = [];    
      opened_his.value = [];
    }, 2000);
        
  }
  
  // 一秒後將 openedCard 清空 (牌面覆蓋回去)
  // window.setTimeout(() => {
  //   openedCard.value = [];
  // }, 1000);

  // console.log("opened_his.value is: " + opened_his.value);
  // console.log("opened_his.length is: " + opened_his.value.length);
  // console.log(opened_his.value[opened_his.value.length - 2]);

  // 翻同一張, 牌面覆蓋回去
  if (opened_his.value[opened_his.value.length - 2] === idx) {

    window.setTimeout(() => {
      opened_his.value.pop();
      opened_his.value.pop();

      openedCard.value = [... opened_his.value];  

    }, 1000);    

  } else if (cards.value[ opened_his.value[opened_his.value.length - 2] ] != cards.value[idx] && try_count % 2 === 0) {

    window.setTimeout(() => {
      opened_his.value.pop();
      opened_his.value.pop();

      // ??? 不加 ... 會錯亂, 觀察 push 到 openedCard 的, 也會到 opened_his 算到 2 次
      openedCard.value = [... opened_his.value];  

    }, 1000);

  } 

  console.log("openedCard's length is: " + openedCard.value.length);

}

const showFinishMsg = () => {
    alert("恭喜破關，再來一局");
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
      
      <div 
        v-for="(n, idx) in cards"
        class="flip-card"
        :class="{
          'open': openedCard.includes(idx)
        }"
        @click="clickHandler(idx)">
        <div class="flip-card-inner" v-if="cards[idx] > 0">
          <div class="flip-card-front"></div>
          <div class="flip-card-back">
            <img :src="`./img/cat-0${n}.jpg`" alt="">
          </div>
        </div>
      </div>
      
    </div>
  </div>
</template>

<style scoped src="./MatchGame.css"></style>