<template>
  <nav v-if="pagerEnd > 0">
    <ul class="pagination">
      <li @click.prevent="$emit('setPage' , pageCurrent - 1)" class="page-item">
        <a class="page-link" href>Previous</a>
      </li>

      <li
        v-for="i in pagerEnd"
        :class="{ active: i + pagerAddAmount === pageCurrent }"
        :key="i"
        @click.prevent="$emit('setPage', i + pagerAddAmount )"
        class="page-item"
      >
        <a class="page-link" href>{{ i + pagerAddAmount }}</a>
      </li>

      <li @click.prevent="$emit('setPage' , pageCurrent + 1)" class="page-item">
        <a class="page-link" href>Next</a>
      </li>
    </ul>
  </nav>
</template>
<script setup>
import { computed } from 'vue';

const props = defineProps({
    pagerEnd : Number,
    currentPage : Number,
    pagerAddAmount: Number
})

const emit = defineEmits(['setPage'])

const pageCurrent = computed(()=>props.currentPage)
</script>
<style>
.pagination {
  display: flex;
  justify-content: center;
}
</style>