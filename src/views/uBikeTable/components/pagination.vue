<script setup>
import { toRefs, ref, computed } from 'vue';

const props = defineProps({
    currentPage: {
        type: Number
    },
    totalPageCount: {
        type: Number
    },
    PAGINATION_MAX: {
        type: Number
    }
})

const { currentPage, totalPageCount } = toRefs(props);

const emit = defineEmits(['updateCurrentPage'])

// const currentPage = ref(props.currentPage);
// const totalPageCount = ref(props.totalPageCount);
const PAGINATION_MAX = props.PAGINATION_MAX;

// 分頁的尾端
const pagerEnd = computed(() => {
    return totalPageCount.value <= PAGINATION_MAX
        ? totalPageCount.value
        : PAGINATION_MAX;
});
// 分頁的位移，用來確保目前的頁碼在中間
const pagerAddAmount = computed(() => {
    const tmp =
        totalPageCount.value <= PAGINATION_MAX
            ? 0
            : currentPage.value + 4 - pagerEnd.value;
    return tmp <= 0
        ? 0
        : totalPageCount.value - (PAGINATION_MAX + tmp) < 0
            ? totalPageCount.value - PAGINATION_MAX
            : tmp;
});
// 換頁
const setPage = page => {
    if (page < 1 || page > totalPageCount.value) {
        return;
    }
    // currentPage.value = page;
    emit('updateCurrentPage', page)
};
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

<style lang="scss" scoped>
.pagination {
  display: flex;
  justify-content: center;
}
</style>