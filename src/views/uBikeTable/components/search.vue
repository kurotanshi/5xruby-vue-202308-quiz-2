<script setup>
import { ref, computed, watch } from 'vue';

const searchText = ref( '' );

const emit = defineEmits( [ 'search' ] );

watch( searchText, () => emit( 'search', { kw : searchText.value } ) );
</script>

<template>
  <p>
    站點名稱搜尋: <input type="text" class="border" v-model="searchText">
  </p>
</template>