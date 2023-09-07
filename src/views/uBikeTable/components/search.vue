<script setup>
import { ref, computed } from 'vue';
const props = defineProps({
    searchText: {
        type: String
    }
})

const emit = defineEmits(['change'])

const searchText = computed({
    get: () => {
        return props.searchText
    }
    ,
    set: (val) => {
        emit('change', val)
    }
})

</script>

<template>
    <p>
        站點名稱搜尋: <input type="text" class="border" v-model.lazy="searchText">
    </p>
</template>