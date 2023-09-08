<script setup>
import { computed } from 'vue';

const props = defineProps({
  searchText: {
    type: String
  }
});

const emit = defineEmits(['searchText']);

const searchText = computed({
  get: () => props.searchText,
  set: newVal => emit('searchText', newVal),
});
</script>

<template>
  <input type="text" class="border" v-model="searchText">
</template>