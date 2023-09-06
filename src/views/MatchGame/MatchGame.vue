<script setup>
import { ref } from 'vue';
import Card from './Card.vue'

// 試完成以下功能：
//  1. 點擊卡片，卡片會翻開 (已完成)
//  2. 點擊兩張不同的卡片，卡片會翻回去 
//  3. 點擊兩張相同的卡片，卡片會消失
//  4. 當所有卡片都消失時，顯示「恭喜破關，再來一局？」的對話框，按下確定後重置遊戲
//  5. 將卡片獨立抽出為 Card.vue 元件

const cards = ref([]);
//const openedCard = ref([]);
const isMatchedCards = ref(0);

// 遊戲初始化，洗牌
const gameInit = () => {
  const numArr = [...new Array(16).keys()].map(i => ++i);
  numArr.sort(() => Math.random() - 0.5);
  cards.value = numArr.map(d => (d % 8) + 1);
  //openedCard.value = [];
  isMatchedCards.value = 0;
}

// const clickHandler = (n, idx) => {
//   openedCard.value.push(idx); //將目前開啟的卡片的index 存入
//   //console.log(n);


//   // 一秒後將 openedCard 清空 (牌面覆蓋回去)
//   window.setTimeout(() => {
//     if (openedCard.value.length == 2) {
//       if (cards.value[openedCard.value[0]] === cards.value[openedCard.value[1]]) {
//         console.log('找到了');
//         //cards.value.find(cards.value[openedCard.value[0]])
//         cards.value[openedCard.value[0]] = -1;
//         isMatchedCards.value++;
//         cards.value[openedCard.value[1]] = -1;
//         isMatchedCards.value++;
//         if(isMatchedCards.value === cards.value.length){
//           let result = confirm('恭喜破關，再來一局？');
//           if(result) gameInit();
//         }
//       }

//     }
//     openedCard.value = [];
//   }, 2000);
// }


const clickHandlerByStart = function(event){
  //alert('click');
  gameInit()
}

const resetGameHandler = (isReset) => {
  if(isReset !== 'undefined' && isReset){
    //gameInit();
    console.log('parents reset');
  }
}

</script>

<template>
  <input type="button" value="測試">
  <Card @click="clickHandlerByStart($event)" :source="cards" @resetGame="resetGameHandler(isReset)"/>
</template>

