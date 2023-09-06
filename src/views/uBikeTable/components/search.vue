<script setup>
import { ref } from 'vue';
import "bootstrap/dist/css/bootstrap.css";

const searchComp = ref('');
const emit = defineEmits(['searchBikeEvt']);

</script>

<template>
    <p>
      站點名稱搜尋: 
      <input type="text" class="border" v-model="searchComp" 
      @keyup.self="$emit('searchBikeEvt', {searchKey:searchComp})">
    </p>
</template>

<style lang="scss" scoped>
</style>