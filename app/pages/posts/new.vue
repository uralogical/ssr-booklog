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
        <el-table
          :data='response.items'
          style='width: 100%'
          height="700"
          class='table'
        >
          <el-table-column
            label='表紙'
          >
            <div slot-scope="{row}" class="img-container">
              <img
                v-if="row.volumeInfo.imageLinks"
                :src="row.volumeInfo.imageLinks.smallThumbnail"
                alt="row.volumeInfo.title"
              >
            </div>
          </el-table-column>
          <el-table-column
            prop='volumeInfo.title'
            label='タイトル'
          >
          </el-table-column>
          <el-table-column
            prop='volumeInfo.authors'
            label='著者'
          >
          </el-table-column>
          <el-table-column>
            <template scope="scope">
              <el-button type='primary' @click='register(scope.$index)' round>
                <span class='el-icon-circle-plus'></span>
                <span>本を登録</span>
              </el-button>
            </template>
          </el-table-column>
        </el-table>
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
    async search() {
      const baseUrl = 'https://www.googleapis.com/books/v1/volumes'
      const searchWord = this.formData.searchWord
      const params = {
        q: searchWord,
        Country: "JP",
        maxResults: 20,
        startIndex: 0,
      };
      this.response = await this.$axios.$get(baseUrl, { params: params });
    },
    async register(index) {
      const bookInfo = this.response.items[index].volumeInfo;

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