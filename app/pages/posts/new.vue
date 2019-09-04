<template>
  <section class='container post-page'>
    <el-card style='flex: 1'>
      <div slot='header' class='clearfix'>
        <el-input placeholder='本を検索' v-model='formData.searchWord' />
        <div class='text-right' style='margin-top: 16px;'>
          <el-button type='primary' @click='search' round>
            <span class='el-icon-search'></span>
            <span>Search</span>
          </el-button>
        </div>
      </div>
      <div v-if="response.length !== 0">
        <el-carousel v-if="response" type="card" :autoplay="false" ref="carousel">
          <el-carousel-item v-for="(item, key) in response.items" :key="key">
            <div
              v-if="item.volumeInfo.imageLinks"
              style="width: 100%; height: 100%; text-align: center"
            >
              <img
                :src="item.volumeInfo.imageLinks.thumbnail"
                class="medium"
                style="height: 100%;"
              >
            </div>
          </el-carousel-item>
        </el-carousel>
        <div slot='header' class='clearfix'>
          <div class='text-center' style='margin-top: 16px;'>
            <el-button type='primary' @click='register' round>
              <span class='el-icon-circle-plus'></span>
              <span>本を登録</span>
            </el-button>
          </div>
        </div>
      </div>
    </el-card>
  </section>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';

export default {
  asyncData({ redirect, store }) {
    if(!store.getters['user']) {
      redirect('/')
    }
    return {
      formData: {
        searchWord: '',
      },
    }
  },
  computed: {
    ...mapGetters(['user'])
  },
  data() {
    return {
      response: []
    }
  },
  methods: {
    async searchRakutenAPI() {
      const baseUrl = 'https://app.rakuten.co.jp/services/api/BooksBook/Search/20170404?'
      const params = {
        applicationId: '1007521135499136307',
        keyword: this.formData.searchWord
      }
      this.response = await this.$axios.$get(baseUrl, { params: params });
      console.log(this.response)
    },
    async search() {
      const baseUrl = 'https://www.googleapis.com/books/v1/volumes'
      const searchWord = this.formData.searchWord
      const params = {
        q: `inauthor:${searchWord}`, // 検索キーワード。intitle:で書籍名が対象に
        Country: "JP",           // 国の指定。JPで日本の指定
        maxResults: 20,          // 取得する検索件数。10~40件を指定可。デフォルトは10件
        startIndex: 0,        // ページングのページ数を指定。0-index
      };
      this.response = await this.$axios.$get(baseUrl, { params: params });
      console.log(this.response)
    },
    async register() {
      console.log(this.response.items[this.$refs.carousel.activeIndex])
      const bookInfo = this.response.items[this.$refs.carousel.activeIndex].volumeInfo;

      const payload = {
        user: this.user,
        title: bookInfo.title,
        authors: bookInfo.authors,
        description: bookInfo.description,
        publisher: bookInfo.publisher,
        pageCount: bookInfo.pageCount,
        thumbnail: bookInfo.imageLinks.thumbnail,
      }
      await this.registerBook({ payload })
      this.$notify({
        type: 'success',
        title: '登録成功',
        message: `${bookInfo.title} を登録しました`,
        position: 'bottom-right',
        duration: 1000
      })
      // this.$router.push('/posts')
    },
    ...mapActions('users', ['updateUser']),
    ...mapActions('posts', ['registerBook'])
  }
}
</script>

<style>
.post-page .el-table__row {
  cursor: pointer;
}

</style>