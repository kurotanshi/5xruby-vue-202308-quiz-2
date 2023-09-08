<script setup>
import { computed } from 'vue';

// 定義 props
const props = defineProps({
  searchText: {
    type: String,
    default: '',
  },
});

// 定義事件
const emit = defineEmits(['update:searchText']);

// 定義 computed, 當 searchText 變動時，會觸發 emit('update:searchText', val)
const searchText = computed({
  get: () => props.searchText,
  set: val => emit('update:searchText', val),
});

</script>

<template>
  <p>
    站點名稱搜尋: <input type="text" class="border" v-model="searchText">
  </p>
</template>
