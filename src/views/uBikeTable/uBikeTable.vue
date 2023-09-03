<script setup>
import { ref, computed, watch } from 'vue';
import "bootstrap/dist/css/bootstrap.css";

import search from './components/search.vue'
import uBikeTable from './components/uBikeTable.vue'
import pagination from './components/pagination.vue'
// 修改這份 YouBike 即時資訊表：
// 1. 將搜尋的部分拆出來變成子元件 `uBikeTable/components/search.vue`
// 2. 將表格的部分拆出來變成子元件 `uBikeTable/components/uBikeTable.vue`
// 3. 將分頁的部分拆出來變成子元件 `uBikeTable/components/pagination.vue`
// 4. 再將它們組合起來

const searchText = ref( '' );

// 目前頁碼
const currentPage = ref( 1 );
// 一頁幾筆資料
const COUNT_OF_PAGE = 20;
// 最多顯示幾頁
const PAGINATION_MAX = 10;

// 分頁的位移，用來確保目前的頁碼在中間
const pagerAddAmount = computed( () =>
                                 {
                                   const tmp =
                                       totalPage.value <= PAGINATION_MAX
                                           ? 0
                                           : currentPage.value + 4 - pageEnd.value;
                                   return tmp <= 0
                                       ? 0
                                       : totalPage.value - ( PAGINATION_MAX + tmp ) < 0
                                           ? totalPage.value - PAGINATION_MAX
                                           : tmp;
                                 } );


// 監聽搜尋文字，若有變動則將頁碼歸 1
watch( searchText, () =>
{
  currentPage.value = 1;
} );

const updateSearch = ( { kw } ) =>
{
  searchText.value = kw;
};

const pageEnd = ref( 1 );
const totalPage = ref( 1 );
const Paging = ( { totalPageCount, nowPage, pagerEnd } ) =>
{
  console.log( 'Parent', 'totalPageCount:' + totalPageCount, 'pagerEnd:' + pagerEnd, 'nowPage:' + nowPage );
  totalPage.value = totalPageCount;
  currentPage.value = nowPage;
  pageEnd.value = pagerEnd;
}

</script>

<template>
  <div class="app">
    <search @search="updateSearch"></search>
    <pagination :totalPageCount="totalPage" :currentPage="currentPage" :pagerEnd="pageEnd" :pagerAddAmount="pagerAddAmount"
                @paging="Paging"></pagination>
    <uBikeTable :kw="searchText" :page="currentPage" :COUNT_OF_PAGE="COUNT_OF_PAGE" :PAGINATION_MAX="PAGINATION_MAX"
                @paging="Paging"
    ></uBikeTable>
  </div>
</template>

<style lang="scss" scoped>
.app {
  padding: 1rem;
}

.pointer {
  cursor: pointer;
}

.pagination {
  display: flex;
  justify-content: center;
}

@media (max-width: 768px) {
  .sno {
    max-width: 50px;
    word-wrap: break-word;
  }
  .table td, .table th {
    padding: .5rem .25rem;
  }
}
</style>