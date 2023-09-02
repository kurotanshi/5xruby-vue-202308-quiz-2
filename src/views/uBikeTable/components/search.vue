<script setup>

import { ref} from 'vue';

const props = defineProps({
    text:String
})


const searchText = ref(props.text);

const emit = defineEmits(['update']);

const setText = () => {
    // searchText.value = text;
    emit('update', searchText.value);
}


</script>

<template>
  <p>
      <!-- 站點名稱搜尋: <input type="text" class="border" v-model="searchText"> -->
      站點名稱搜尋: <input type="text" class="border" v-model="searchText" @input="setText()"/>
    </p>
</template>
