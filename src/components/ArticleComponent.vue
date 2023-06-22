<script setup>
import { ref,reactive } from 'vue';
import { defineProps } from 'vue';

// 親から受け取るデータ
const props = defineProps(['article'])

</script>

<template>
    <div class="article">
        <a :href="props.article.url">
            <h2>{{ props.article.title }}</h2>
        </a>
    </div>
</template>

<style scoped lang="scss">
.article{
    border: 1px dashed #000000;
    margin: 0.5rem;
    padding:0.5rem;

}
h4{margin: 0.5rem 0 ;}
</style>