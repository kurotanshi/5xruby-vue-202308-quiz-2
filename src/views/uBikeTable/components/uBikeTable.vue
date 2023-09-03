<script setup>
// 欄位說明:
// https://data.taipei/dataset/detail?id=c6bc8aed-557d-41d5-bfb1-8da24f78f2fb
// sno：站點代號、 sna：場站名稱(中文)、 tot：場站總停車格、
// sbi：場站目前車輛數量、 sarea：場站區域(中文)、 mday：資料更新時間、
// lat：緯度、 lng：經度、 ar：地(中文)、 sareaen：場站區域(英文)、
// snaen：場站名稱(英文)、 aren：地址(英文)、 bemp：空位數量、 act：全站禁用狀態

import { ref, computed, watch } from 'vue';

const emit = defineEmits( [ 'paging' ] );

const props = defineProps( {
                             kw : String,
                             page : Number,
                             COUNT_OF_PAGE : Number,
                             PAGINATION_MAX : Number
                           } );

// 所有站點資料
const uBikeStops = ref( [] );
// 搜尋文字
const searchText = ref( props.kw );
// 目前的排序選項
const currentSort = ref( 'sbi' );
// 是否為降冪排序
const isSortDesc = ref( false );
// 目前頁碼
const currentPage = ref( props.page );
// 一頁幾筆資料
const COUNT_OF_PAGE = ref( props.COUNT_OF_PAGE );
// 最多顯示幾頁
const PAGINATION_MAX = ref( props.PAGINATION_MAX );

fetch( 'https://tcgbusfs.blob.core.windows.net/dotapp/youbike/v2/youbike_immediate.json' )
    .then( res => res.text() )
    .then( data =>
           {
             uBikeStops.value = JSON.parse( data );
           } );

// 篩選後的站點資料
const filtedUbikeStops = computed( () =>
                                   {
                                     return uBikeStops.value.length === 0
                                         ? []
                                         : uBikeStops.value.filter( d => d.sna.includes( searchText.value ) );
                                   } );
// 排序後的站點資料
const sortedUbikeStops = computed( () =>
                                   {
                                     const filtedStops = [ ...filtedUbikeStops.value ];
                                     return isSortDesc.value
                                         ? filtedStops.sort( ( a, b ) => b[ currentSort.value ] - a[ currentSort.value ] )
                                         : filtedStops.sort( ( a, b ) => a[ currentSort.value ] - b[ currentSort.value ] );
                                   } );

// 指定排序
const setSort = sortType =>
{
  if ( sortType === currentSort.value )
  {
    isSortDesc.value = !isSortDesc.value;
  }
  else
  {
    currentSort.value = sortType;
    isSortDesc.value = false;
  }
};

// 總頁數
const totalPageCount = computed( () =>
                                 {
                                   return Math.ceil( filtedUbikeStops.value.length / COUNT_OF_PAGE.value );
                                 } );
// 分頁的尾端
const pagerEnd = computed( () =>
                           {
                             return totalPageCount.value <= PAGINATION_MAX.value
                                 ? totalPageCount.value
                                 : PAGINATION_MAX.value;
                           } );

// 分頁後的站點資料
const slicedUbikeStops = computed( () =>
                                   {
                                     const start = ( currentPage.value - 1 ) * COUNT_OF_PAGE.value;
                                     const end =
                                         start + COUNT_OF_PAGE.value <= sortedUbikeStops.value.length
                                             ? start + COUNT_OF_PAGE.value
                                             : sortedUbikeStops.value.length;
                                     return sortedUbikeStops.value.slice( start, end );
                                   } );

watch( slicedUbikeStops, () => emit( 'paging', { totalPageCount : totalPageCount.value, nowPage: currentPage.value, pagerEnd : pagerEnd.value } ) );

const keywordsHighlight = ( text, keyword ) =>
{
  const reg = new RegExp( keyword, 'gi' );
  return text.replace( reg, `<span style="color: red;">${ keyword }</span>` );
};

const timeFormat = ( val ) =>
{
  // 時間格式
  const pattern = /(\d{4})(\d{2})(\d{2})(\d{2})(\d{2})(\d{2})/;
  return val.replace( pattern, '$1/$2/$3 $4:$5:$6' );
};

watch( () => [ props.kw, props.page ], ( [ kw, page ] ) =>
{
  currentPage.value = searchText.value !== kw ? 1 : page;
  searchText.value = kw;
} );
</script>

<template>
  <table class="table table-striped">
    <thead>
    <tr>
      <th @click="setSort('sno')">
        #
        <span v-show="currentSort === 'sno'">
              <i class="fa" :class="isSortDesc ? 'fa-sort-desc' : 'fa-sort-asc'" aria-hidden="true"></i>
        </span>
      </th>
      <th>
        場站名稱
      </th>
      <th>
        場站區域
      </th>
      <th @click="setSort('sbi')" class="pointer">
        目前可用車輛
        <span v-show="currentSort === 'sbi'">
              <i class="fa" :class="isSortDesc ? 'fa-sort-desc' : 'fa-sort-asc'" aria-hidden="true"></i>
        </span>
      </th>
      <th @click="setSort('tot')" class="pointer">
        總停車格
        <span v-show="currentSort === 'tot'">
              <i class="fa" :class="isSortDesc ? 'fa-sort-desc' : 'fa-sort-asc'" aria-hidden="true"></i>
            </span>
      </th>
      <th>
        資料更新時間
      </th>
    </tr>
    </thead>
    <tbody>
    <!-- 替換成 slicedUbikeStops -->
    <tr v-for="s in slicedUbikeStops" :key="s.sno">
      <td>{{ s.sno }}</td>
      <!-- <td>{{ s.sna }}</td> -->
      <td v-html="keywordsHighlight(s.sna, searchText)"></td>
      <td>{{ s.sarea }}</td>
      <td>{{ s.sbi }}</td>
      <td>{{ s.tot }}</td>
      <td>{{ timeFormat( s.mday ) }}</td>
    </tr>
    </tbody>
  </table>
</template>

<style scoped>

</style>