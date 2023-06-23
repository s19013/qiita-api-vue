<script setup>
import {ref,defineProps,watch } from 'vue';

const selected = ref(props.original)

/** 
 * @param {original} 初期値
 * @param {options} optionタグの情報
*/
const props = defineProps({
    original:{
        type:[ String, Number],
        required: true
    },
    options:{
        type:Array,
        required: true
    }
})

/**  親が子の情報を受け取る */
const passSelected = () => {
    return selected.value
}

/**  propsの値が変化したらselectedを更新する */
watch(() => props.original,(newValue,oldValue) => {
  selected.value = newValue
})

defineExpose({
    passSelected
});
</script>

<template>
    <div class="selector">
        <select v-model="selected">
            <option v-for="(option,index) in options" :key="index" :value="option">
                {{ option }}
            </option>
        </select>
    </div>
</template>

<style scoped lang="scss">

</style>