<script setup>
import { ref, computed, watch } from 'vue';

const emit = defineEmits( [ 'paging' ] );

const props = defineProps( {
                             totalPageCount : Number,
                             currentPage : Number,
                             pagerEnd : Number,
                             pagerAddAmount : Number
                           } );

const totalPageCount = ref( props.totalPageCount );
const currentPage = ref( props.currentPage );
const pagerEnd = ref( props.pagerEnd );
const pagerAddAmount = ref( props.pagerAddAmount );
// 換頁
const setPage = page =>
{
  if ( page < 1 || page > totalPageCount.value )
  {
    return;
  }
  currentPage.value = page;

  emit( 'paging', { totalPageCount : totalPageCount.value, nowPage : currentPage.value, pagerEnd : pagerEnd.value } )
};

watch( () => [ props.totalPageCount, props.currentPage, props.pagerEnd, props.pagerAddAmount ], ( [a, b, c, d] ) => {
  totalPageCount.value = a;
  currentPage.value = b;
  pagerEnd.value = c;
  pagerAddAmount.value = d;
} )
</script>

<template>
  <nav v-if="pagerEnd > 0">
    <ul class="pagination">
      <li @click.prevent="setPage(currentPage - 1)" class="page-item">
        <a class="page-link" href>Previous</a>
      </li>

      <li v-for="i in pagerEnd" :class="{ active: i + pagerAddAmount === currentPage }" :key="i"
          @click.prevent="setPage(i + pagerAddAmount)" class="page-item">
        <a class="page-link" href>{{ i + pagerAddAmount }}</a>
      </li>

      <li @click.prevent="setPage(currentPage + 1)" class="page-item">
        <a class="page-link" href>Next</a>
      </li>
    </ul>
  </nav>
</template>
