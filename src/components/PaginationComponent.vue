<script setup>
import {ref,defineProps,watch,onMounted,nextTick,defineEmits } from 'vue';

const selected = ref(props.original)
const displayNumber = ref([])

/**
 * @param original 初期値
 * @param pageCount ページ数
 */
const props = defineProps({
    original:{
        type:Number,
        default:1,
    },
    pageCount:{
        type:Number,
        default:100
    }
})

const emit = defineEmits(['turnPage'])

/** @func displayNumberを作り直す
 *  @param min 表示上の最小値
 *  @param max 表示上の最大値
*/
const rebuildingDisplayNumber = (min,max) => {
    const temp = []
    for (let index = min; index <= max; index++) {
        temp.push(index)
    }

    displayNumber.value = temp
}

// ページをめくる(親コンポーネントに連絡)
const turnPage = (num) => {
    emit('turnPage',num)
}

// propsの値が変化したらselectedを更新する
watch(() => props.original,(newValue,oldValue) => {
    // newValueが5以下の場合表示の仕方が特殊
    selected.value = newValue
    if (newValue <= 5 ) {rebuildingDisplayNumber(1,9)}
    else{
        rebuildingDisplayNumber(selected.value - 4,selected.value + 4)
    }
})

onMounted(() => { 
  // selectedが5以下の場合表示の仕方が特殊
  nextTick(() => {
    if (selected.value <= 5 ) {rebuildingDisplayNumber(1,9)}
    else {rebuildingDisplayNumber(selected.value - 4,selected.value + 4)}
  })
});

defineExpose({
    
});
</script>

<template>
    <div class="pagination">
        <ul>
            <li v-for="num in displayNumber" :key="num">
                <button 
                    :class="num == selected ? 'selected': ''" 
                    :disabled="num == selected"
                    @click="turnPage(num)"
                >
                    {{ num }}
                </button>
            </li>
        </ul>
    </div>
</template>

<style scoped lang="scss">
ul{display: flex;}
li{
    list-style: none;
    button{
        padding: 0.5rem  1rem;
    }
}
.selected{
    background-color: aquamarine;
}
</style>