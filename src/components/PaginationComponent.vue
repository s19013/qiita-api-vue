<script setup>
import {ref,defineProps,watch,onMounted,defineEmits } from 'vue';

const selected = ref(props.original)
const displayPages = ref([])

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

/** @func displayPagesを作り直す
 *  @param min 表示上の最小値
 *  @param max 表示上の最大値
*/
const rebuildingDisplayPages = (min,max) => {
    const temp = []
    for (let index = min; index <= max; index++) {
        temp.push(index)
    }

    displayPages.value = temp
}

// ページをめくる(親コンポーネントに連絡)
const turnPage = (num) => { emit('turnPage',num) }

// watchとmountedで使う処理
const divide = () => {
    // props...とか書くのめんどいので変数に入れとく
    const pageCount = props.pageCount.value

    // selectedが5以下の場合表示の仕方が特殊
    if (selected.value <= 5 ) {rebuildingDisplayPages(1,9)}
    // pageCountに近い時も特殊
    else if (selected.value > pageCount) {
        rebuildingDisplayPages(pageCount - 8,pageCount)
    }
    else {rebuildingDisplayPages(selected.value - 4,selected.value + 4)}
}

// propsの値が変化したらselectedを更新する
watch(() => props.original,(newValue,oldValue) => {
    selected.value = newValue
    divide()
})

onMounted(() => { divide() });
</script>

<template>
    <div class="pagination">
        <ul>
            <li v-for="num in displayPages" :key="num">
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
    button{ padding: 0.5rem  1rem; }
}
.selected{ background-color: aquamarine; }
</style>