<script setup>
import {ref,defineProps,watch } from 'vue';

const props = defineProps({
    original:{
        type:String,
        default:null,
    }
})
const keyword = ref(props.original)
// 親が子の情報を受け取る
const passKeyword = () => {
    return keyword.value
}

// propsの値が変化したらkeywordを更新する
watch(origin,(newValue,oldValue) => {
  keyword.value = newValue
})


defineExpose({
    passKeyword
});
</script>

<template>
    <input type="text" v-model="keyword"/>
</template>

<style lang="scss" scope>
</style>