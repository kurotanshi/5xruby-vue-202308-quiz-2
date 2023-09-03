<script setup>
import { computed } from 'vue';

const props = defineProps({ modelValue: String });
const emit = defineEmits(['update:modelValue']);

const searchText = computed({
  get() {
    return props.modelValue;
  },
  set(newVal) {
    emit('update:modelValue', newVal);
  },
});
</script>

<template>
  <p>站點名稱搜尋: <input type="text" class="border" v-model="searchText" /></p>
</template>
