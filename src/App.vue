<script setup>
import axios from 'axios';
import { ref,reactive,onMounted,nextTick } from 'vue';
import ArticleComponent from './components/ArticleComponent.vue';

const articles = ref([]);


/** qiitaの記事取ってくる */
const getArticle = async() => {
  await axios.get('https://qiita.com/api/v2/items',{
    params:{
      page:1,
      per_page:10,
      query:null,
    }
  })
  /** @param {res} api通信で取ってきたデータたち */
  .then((res)=>{
      // 配列を入れ替え
      articles.value = res.data
  })
  .catch((errors) => {
    console.log(errors);
  })
}

// onMounted(() => { 
//   getArticle()
// });
</script>

<template>
  <main>
    <template v-for="(article,index) in articles" :key="index">
      <ArticleComponent
        :article="article"
      />
    </template>
  </main>
</template>

<style scoped lang="scss">
.main{
  width: 100%;
}
</style>
