<script setup>
import { computed } from 'vue';

const props = defineProps({
  totalPageCount: {
    type: Number
  },
  currentPage: {
    type: Number
  },
  paginationMax: {
    type: Number
  },
});

const emit = defineEmits(['currentPage']);

const currentPage = computed({
  get: () => props.currentPage,
  set: newVal => emit('currentPage', newVal),
});

// 分頁的尾端
const pagerEnd = computed(() => {
  return props.totalPageCount <= props.paginationMax
    ? props.totalPageCount
    : props.paginationMax;
});

// 分頁的位移，用來確保目前的頁碼在中間
const pagerAddAmount = computed(() => {
  const tmp =
    props.totalPageCount <= props.paginationMax
      ? 0
      : currentPage.value + 4 - pagerEnd.value;
  return tmp <= 0
    ? 0
    : props.totalPageCount - (props.paginationMax + tmp) < 0
      ? props.totalPageCount - props.paginationMax
      : tmp;
});

// 換頁
const setPage = page => {
  if (page < 1 || page > props.totalPageCount) {
    return;
  }
  currentPage.value = page;
};

</script>

<template>
  <nav v-if="pagerEnd > 0">
    <ul class="pagination">
      <li @click.prevent="setPage(props.currentPage - 1)" class="page-item">
        <a class="page-link" href>Previous</a>
      </li>

      <li v-for="i in pagerEnd" :class="{ active: i + pagerAddAmount === props.currentPage }" :key="i"
        @click.prevent="setPage(i + pagerAddAmount)" class="page-item">
        <a class="page-link" href>{{ i + pagerAddAmount }}</a>
      </li>

      <li @click.prevent="setPage(props.currentPage + 1)" class="page-item">
        <a class="page-link" href>Next</a>
      </li>
    </ul>
  </nav>
</template>

<style lang="scss" scoped>
.pointer {
  cursor: pointer;
}
.pagination {
  display: flex;
  justify-content: center;
}
</style>