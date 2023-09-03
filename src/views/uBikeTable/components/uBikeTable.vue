<script setup>
const props = defineProps({
  currentSort: String,
  isSortDesc: Boolean,
  searchText: String,
  slicedUbikeStops: Array,
});

const emit = defineEmits(['setSort']);

const setSort = (column) => emit('setSort', column);

// 關鍵字 Highlight
const keywordsHighlight = (text, keyword) => {
  const reg = new RegExp(keyword, 'gi');
  return text.replace(reg, `<span style="color: red;">${keyword}</span>`);
};
</script>

<template>
  <table class="table table-striped">
    <thead>
      <tr>
        <th @click="setSort('sno')">
          #
          <span v-show="props.currentSort === 'sno'">
            <i
              class="fa"
              :class="props.isSortDesc ? 'fa-sort-desc' : 'fa-sort-asc'"
              aria-hidden="true"
            ></i>
          </span>
        </th>
        <th>場站名稱</th>
        <th>場站區域</th>
        <th @click="setSort('sbi')" class="pointer">
          目前可用車輛
          <span v-show="props.currentSort === 'sbi'">
            <i
              class="fa"
              :class="props.isSortDesc ? 'fa-sort-desc' : 'fa-sort-asc'"
              aria-hidden="true"
            ></i>
          </span>
        </th>
        <th @click="setSort('tot')" class="pointer">
          總停車格
          <span v-show="props.currentSort === 'tot'">
            <i
              class="fa"
              :class="props.isSortDesc ? 'fa-sort-desc' : 'fa-sort-asc'"
              aria-hidden="true"
            ></i>
          </span>
        </th>
        <th>資料更新時間</th>
      </tr>
    </thead>
    <tbody>
      <!-- 替換成 slicedUbikeStops -->
      <tr v-for="s in props.slicedUbikeStops" :key="s.sno">
        <td>{{ s.sno }}</td>
        <!-- <td>{{ s.sna }}</td> -->
        <td v-html="keywordsHighlight(s.sna, props.searchText)"></td>
        <td>{{ s.sarea }}</td>
        <td>{{ s.sbi }}</td>
        <td>{{ s.tot }}</td>
        <td>{{ s.mday }}</td>
      </tr>
    </tbody>
  </table>
</template>

<style lang="scss" scoped>
.pointer {
  cursor: pointer;
}

@media (max-width: 768px) {
  .table td,
  .table th {
    padding: 0.5rem 0.25rem;
  }
}
</style>
