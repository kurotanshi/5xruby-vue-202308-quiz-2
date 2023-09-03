<script setup>
const props = defineProps({
  pagerEnd: Number,
  currentPage: Number,
  pagerAddAmount: Number,
});

const emit = defineEmits(['setPage']);

const setPage = (page) => emit('setPage', page);
</script>

<template>
  <nav v-if="props.pagerEnd > 0">
    <ul class="pagination">
      <li @click.prevent="setPage(props.currentPage - 1)" class="page-item">
        <a class="page-link" href>Previous</a>
      </li>

      <li
        v-for="i in props.pagerEnd"
        :class="{ active: i + props.pagerAddAmount === props.currentPage }"
        :key="i"
        @click.prevent="setPage(i + props.pagerAddAmount)"
        class="page-item"
      >
        <a class="page-link" href>{{ i + props.pagerAddAmount }}</a>
      </li>

      <li @click.prevent="setPage(props.currentPage + 1)" class="page-item">
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
