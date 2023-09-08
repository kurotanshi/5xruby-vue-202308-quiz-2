<script setup>
import { ref, computed, watch } from 'vue';
import "bootstrap/dist/css/bootstrap.css";
import '@fortawesome/fontawesome-free/css/all.css';


// 修改這份 YouBike 即時資訊表：
// 1. 將搜尋的部分拆出來變成子元件 `uBikeTable/components/search.vue`
// 2. 將表格的部分拆出來變成子元件 `uBikeTable/components/uBikeTable.vue`
// 3. 將分頁的部分拆出來變成子元件 `uBikeTable/components/pagination.vue`
// 4. 再將它們組合起來

import SearchText from './components/Search.vue';
import uBikeTable from './components/uBikeTable.vue';
import Pagination from './components/Pagination.vue';


// 欄位說明:
// https://data.taipei/dataset/detail?id=c6bc8aed-557d-41d5-bfb1-8da24f78f2fb
// sno：站點代號、 sna：場站名稱(中文)、 tot：場站總停車格、
// sbi：場站目前車輛數量、 sarea：場站區域(中文)、 mday：資料更新時間、
// lat：緯度、 lng：經度、 ar：地(中文)、 sareaen：場站區域(英文)、
// snaen：場站名稱(英文)、 aren：地址(英文)、 bemp：空位數量、 act：全站禁用狀態

// 所有站點資料
const uBikeStops = ref([]);

// 搜尋文字
const searchText = ref('');

// 目前頁碼
const currentPage = ref(1);

// 一頁幾筆資料
const COUNT_OF_PAGE = 20;

fetch('https://tcgbusfs.blob.core.windows.net/dotapp/youbike/v2/youbike_immediate.json')
  .then(res => res.text())
  .then(data => {
    uBikeStops.value = JSON.parse(data);
  });

// 監聽搜尋文字，若有變動則將頁碼歸 1
watch(searchText, () => {
  currentPage.value = 1;
});

// 篩選後的站點資料
const filtedUbikeStops = computed(() => {
  return uBikeStops.value.length === 0
    ? []
    : uBikeStops.value.filter(d => d.sna.includes(searchText.value));
});

// 分頁後的站點資料
const slicedUbikeStops = computed(() => {
  const start = (currentPage.value - 1) * COUNT_OF_PAGE;
  const end =
    start + COUNT_OF_PAGE <= filtedUbikeStops.value.length
      ? start + COUNT_OF_PAGE
      : filtedUbikeStops.value.length;
  return filtedUbikeStops.value.slice(start, end);
});

// 總頁數
const totalPageCount = computed(() => {
  return Math.ceil(filtedUbikeStops.value.length / COUNT_OF_PAGE);
});

// 換頁
const setPage = page => {
  if (page < 1 || page > totalPageCount.value) {
    return;
  }
  currentPage.value = page;
};

</script>

<template>
  <div class="app">
    
    <!-- SearchText 元件 -->
    <SearchText 
      :searchText="searchText" 
      @update:searchText="(val) => searchText = val" />

    <uBikeTable 
      :slicedUbikeStops="slicedUbikeStops"
      :searchText="searchText"
    />

  </div>

  <!-- 頁籤 -->
  <Pagination
    :currentPage="currentPage"
    :totalPageCount="totalPageCount"
    @update:currentPage="setPage"
  />

</template>

<style lang="scss" scoped>
.app {
  padding: 1rem;
}
.pointer {
  cursor: pointer;
}

@media (max-width: 768px) {
  .sno {
    max-width: 50px; word-wrap: break-word;
  }
  .table td, .table th {
    padding: .5rem .25rem;
  }
}
</style>