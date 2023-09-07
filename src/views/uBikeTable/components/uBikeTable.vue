<template>
    <table class="table table-striped">
      <thead>
        <tr>
          <th @click="$emit('desc','sno')">
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
          <th @click="$emit('desc','sbi')" class="pointer">
            目前可用車輛
            <span v-show="currentSort === 'sbi'">
              <i class="fa" :class="isSortDesc ? 'fa-sort-desc' : 'fa-sort-asc'" aria-hidden="true"></i>
            </span>
          </th>
          <th @click="$emit('desc','tot')" class="pointer">
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
          <td v-html="keywordsHighlight(s.sna, search)"></td>
          <td>{{ s.sarea }}</td>
          <td>{{ s.sbi }}</td>
          <td>{{ s.tot }}</td>
          <td>{{ timeFormat(s.mday) }}</td>
        </tr>
      </tbody>
    </table>
</template>
<script setup>
import { computed } from 'vue';

const props =defineProps({
    slicedUbikeStops:Object,
    isSortDesc: Boolean,
    currentSort : String,
    searchText: String
})

const emit =defineEmits(['desc'])

const search = computed(()=>props.searchText)

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
<style>

</style>