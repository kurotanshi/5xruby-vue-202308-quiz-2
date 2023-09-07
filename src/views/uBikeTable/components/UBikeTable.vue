<script setup>
import { computed } from 'vue';

const props = defineProps({
  uBikeStops: {
    type: Array
  },
  currentSort: {
    type: String
  },
  isSortDesc: {
    type: Boolean
  },
  currentPage: {
    type: Number
  },
  countOfPage: {
    type: Number
  },
  searchText: {
    type: String
  }
});

const emit = defineEmits(['currentSort', 'isSortDesc']);

// 目前的排序選項
const currentSort = computed({
  get: () => props.currentSort,
  set: newVal => emit('currentSort', newVal),
});
// 是否為降冪排序
const isSortDesc = computed({
  get: () => props.isSortDesc,
  set: newVal => emit('isSortDesc', newVal),
});

// 指定排序
const setSort = sortType => {
  if (sortType === currentSort.value) {
    isSortDesc.value = !isSortDesc.value;
  } else {
    currentSort.value = sortType;
    isSortDesc.value = false;
  }
};
// 關鍵字 Highlight
const keywordsHighlight = (text, keyword) => {
  const reg = new RegExp(keyword, 'gi');
  return text.replace(reg, `<span style="color: red;">${keyword}</span>`);
};
const timeFormat = (val) => {
  // 時間格式
  const pattern = /(\d{4})(\d{2})(\d{2})(\d{2})(\d{2})(\d{2})/;
  return val.replace(pattern, '$1/$2/$3 $4:$5:$6');
};

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
      <tr v-for="s in props.uBikeStops" :key="s.sno">
        <td>{{ s.sno }}</td>
        <!-- <td>{{ s.sna }}</td> -->
        <td v-html="keywordsHighlight(s.sna, searchText)"></td>
        <td>{{ s.sarea }}</td>
        <td>{{ s.sbi }}</td>
        <td>{{ s.tot }}</td>
        <td>{{ timeFormat(s.mday) }}</td>
      </tr>
    </tbody>
  </table>
</template>

<style lang="scss" scoped>
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