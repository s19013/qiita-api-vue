<script setup>
import axios from 'axios';
import { ref,reactive,onMounted,nextTick } from 'vue';
import ArticleComponent from './components/ArticleComponent.vue';
import InputComponent from './components/InputComponent.vue';
import SelectorComponent from './components/SelectorComponent.vue';
import PaginationComponent from './components/PaginationComponent.vue';

// コンポーネントたち
const Input =ref(null)
const PerPageSelector = ref(null)



const articles = ref([]);
const InputHistory = reactive({
  page:1,
  per_page:10,
  keyword:null
})

/** qiitaの記事取ってくる */
const getArticle = async({page,per_page,keyword}) => {
  console.log(per_page,keyword);
  await axios.get('https://qiita.com/api/v2/items',{
    params:{
      page:page,
      per_page:per_page,
      query:keyword,
    }
  })
  /** @param {res} api通信で取ってきたデータたち */
  .then((res)=>{
      // 配列を入れ替え
      articles.value = res.data

      // 履歴更新
      Object.assign(InputHistory,{ 
        page:page,
        per_page:per_page,
        keyword:keyword
      });
  })
  .catch((errors) => {
    console.log(errors);
  })
}

const search = () =>{
  getArticle({
    page:1,
    per_page:PerPageSelector.value.passSelected(),
    keyword:Input.value.passKeyword()
  })
}

const turnPage = (num) => {
  getArticle({
    page:num,
    per_page:InputHistory.per_page,
    keyword:InputHistory.keyword
  })
}

onMounted(() => { 
  getArticle({
    page:1,
    per_page:InputHistory.per_page,
    keyword:InputHistory.keyword
  })
});
</script>

<template>
  <main>
    <p>1時間につき60回まで</p>
    <div class="search">
      <InputComponent ref="Input" :original="InputHistory.keyword"/>
      <button @click="search">検索</button>
    </div>

    <SelectorComponent 
      ref="PerPageSelector" 
      :original="InputHistory.per_page" 
      :options=[5,10,15,20,25,30] 
    />
    <template v-for="(article,index) in articles" :key="index">
      <ArticleComponent
        :article="article"
      />
    </template>
    <!-- qiitaapiでは100ページ目までしか見れないらしいので -->
    <PaginationComponent 
      :original="InputHistory.page" 
      :pageCount="100"
      @turnPage="turnPage"
    />
  </main>
</template>

<style scoped lang="scss">
.main{

}
</style>
