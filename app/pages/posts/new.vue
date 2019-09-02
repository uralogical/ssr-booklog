<template>
  <section class='container post-page'>
    <el-card style='flex: 1'>
      <div slot='header' class='clearfix'>
        <el-input placeholder='本を検索' v-model='formData.searchWord' />
      </div>
      <el-carousel v-if="response" type="card" :autoplay="false">
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
      <div class='text-right' style='margin-top: 16px;'>
        <el-button type='primary' @click='publish' round>
          <span class='el-icon-upload2'></span>
          <span>Publish</span>
        </el-button>
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
    async publish() {
      const baseUrl = 'https://www.googleapis.com/books/v1/volumes'
      const searchWord = this.formData.searchWord
      const params = {
        q: `intitle:${searchWord}`, // 検索キーワード。intitle:で書籍名が対象に
        Country: "JP",           // 国の指定。JPで日本の指定
        maxResults: 20,          // 取得する検索件数。10~40件を指定可。デフォルトは10件
        startIndex: 0,        // ページングのページ数を指定。0-index
      };
      this.response = await this.$axios.$get(baseUrl, { params: params });
      console.log(this.response)
      // const payload = {
      //   user: this.user,
      //   ...this.formData,
      // }
      // await this.publishPost({ payload })
      // this.$router.push('/posts')
    },
    ...mapActions('users', ['updateUser']),
    ...mapActions('posts', ['publishPost'])
  }
}
</script>

<style>
.post-page .el-table__row {
  cursor: pointer;
}

</style>