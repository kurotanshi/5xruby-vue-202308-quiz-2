<script setup>
import { ref } from 'vue';
import "bootstrap/dist/css/bootstrap.css";

const emit = defineEmits(['setPageEvt']);

const props = defineProps(
    {
        pagerEnd:Number,
        currentPage: {
            type: Number,
            default: 1
        },
        pagerAddAmount:Number,
    }
)

</script>

<template>

  <button @click.self="$emit('setPageEvt')"> Test </button>

  <!-- 頁籤 -->
  <nav v-if="props.pagerEnd > 0">
    <ul class="pagination">
      <li class="page-item"
      @click.prevent="$emit('setPageEvt', 
      {currentPageKey:props.currentPage-1})">
        <a class="page-link" href>Previous</a>
      </li>

      <li v-for="i in props.pagerEnd" :class="{ active: i + props.pagerAddAmount === props.currentPage }" :key="i"
        @click.prevent="$emit('setPageEvt',{currentPageKey:props.pagerAddAmount+i})" 
        class="page-item">
        <a class="page-link" href>{{ i + props.pagerAddAmount }}</a>
      </li>

      <li class="page-item"
      @click.prevent="$emit('setPageEvt', 
      {currentPageKey:props.currentPage+1})">
        <a class="page-link" href>Next</a>
      </li>
    </ul>
  </nav>   

</template>

<style lang="scss" scoped>
.pagination {
  display: flex;
  justify-content: center;
}

</style>