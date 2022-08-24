<template>
  <div>
    <div class="ui main container">
      <!-- 基本的なコンテンツはここに記載する -->
      <div class="ui segment">
        <form class="ui form" @submit.prevent="postArticle">
          <div class="field">
            <textarea type="text" placeholder="記事" v-model="post.text" >
            </textarea>
          </div>
          <div class="field" style="align-items: center;">
            <div class="inline fields">
              <div class="field">
                <label>カテゴリー</label>
                <input type="text" v-model="post.category" />
              </div>
              <button class="ui button" type="submit">
                投稿
              </button>
            </div>
          </div>
        </form>
      </div>
      <div class="ui segment">
        <div class="field">
          <form class="ui form" @submit.prevent="searchArticle">
              <div class="field">
                <label>ユーザー名</label>
                <input type="text" v-model="search.text" />
              </div>
              <div class="field">
                <label>カテゴリー名</label>
                <input type="text" v-model="search.text" />
              </div>
              <div class="field">
                <label>投稿日時</label>
                <div class="inline fields">
                  <div class="field">
                    <input type="date" v-model="search.text" />
                    <label>から</label>
                  </div>
                  <div class="field">
                    <input type="date" v-model="search.text" />
                    <label>まで</label>
                  </div>
                </div>
              </div>
            <button class="ui button" type="submit" style="width: 100%">検索</button>
          </form>
        </div>
      </div>
      <template v-for="(article, index) in sortedArticles">
        <div class="ui segment" :key="index">
          <form class="ui form">
            <h2 class="ui header">
              <div class="content">
                {{article.userId}}
                 <button class="ui red label" type="submit" >削除</button>
              </div>
              <div class="sub header">{{article.text}}</div>
            </h2>
            <div class="ui green label">{{article.category}}</div>
          </form>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
import { baseUrl } from '@/assets/config.js';
import axios from "axios";
// 必要なものはここでインポートする
// @は/srcと同じ意味です
// import something from '@/components/something.vue';

export default {
  name: 'Home',
  components: {
    // 読み込んだコンポーネント名をここに記述する
  },
  data() {
    // Vue.jsで使う変数はここに記述する
    return {
      post: {
        text: null,
        category: null,
        err: null
      },
      search: {
        username: null,
        category: null,
        start: null,
        end: null,
        err: null,
      },
      query: {
        start: null,
        end: null
      },
      articles: [],
      iam: null
    };
  },
  computed: {
  // 計算した結果を変数として利用したいときはここに記述する
    sortedArticles() {
      let result = this.articles;
      // if(this.query.start && this.query.end) {
      //   serachArticle();
      // }
      return result;
    },
  },
  created() {
    // Vue.jsの読み込みが完了したときに実行する処理はここに記述する
    this.iam = localStorage.getItem("userId");
    // apiからarticleを取得する
    if (!window.localStorage.getItem("userId")) {
      this.$router.push({name: "Login"});
    }
    this.getArticles();
  },
  methods: {
    // Vue.jsで使う関数はここで記述する
    getArticles() {
      axios.get(baseUrl + "/articles")
        .then((res) => {
          this.articles = res.data;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    /*
    postArticle() {
    },
    searchArticle() {
    },
    deleteArticle(article) {
    },
    fromUnixtoDate(timestamp) {
    }
    */
  }
}
</script>
<style scoped>
</style>