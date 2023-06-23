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
    // QiitaApiではqueryが空文字だとなにも取ってこないので回避
    if (keyword.value == '') {keyword.value = null}
    return keyword.value
}

// propsの値が変化したらkeywordを更新する
// そのままprops.originalだと動かないらしい
// おそらくアロー関数使って帰り値で判断してるのかも?
watch( () => props.original,(newValue,oldValue) => {
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