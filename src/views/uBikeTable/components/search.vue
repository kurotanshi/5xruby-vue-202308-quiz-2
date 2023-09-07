<template>
    <p>
        站點名稱搜尋: <input type="text" class="border" v-model="search" >
    </p>
</template>
<script setup>
import { computed } from 'vue';

const props =defineProps({
    searchText:String
})

// const search =ref(props.searchText)

// 1.在script用要先註冊事件

// const emit =defineEmits(['update'])
// const searchFunc = () =>{
//     emit('update',search.value)
// }

// 2.在input上寫@input=$emit('update',search)

const emit =defineEmits(['update'])

const search = computed({
    get: ()=>props.searchText,
    set: (val)=>{
        emit('update',val)
    }
})

</script>
