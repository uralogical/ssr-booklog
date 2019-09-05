<template>
  <section class='container post-page'>
    <el-card style='flex: 1'>
      <div slot='header' class='clearfix'>
        <span>読んだ本の一覧</span>
      </div>
      <el-carousel type="card" :autoplay="false" ref="carousel">
        <el-carousel-item v-for="(item, key) in showPosts" :key="key">
          <div
            style="width: 100%; height: 100%; text-align: center"
          >
            <img
              :src="item.thumbnail"
              class="medium"
              style="height: 100%;"
            >
          </div>
        </el-carousel-item>
      </el-carousel>
    </el-card>
  </section>
</template>

<script>
import moment from '~/plugins/moment'
import { mapGetters } from 'vuex'

export default {
  async asyncData({ store }) {
    await store.dispatch('posts/fetchPosts')
  },
  computed: {
    showPosts() {
      return this.posts.map(post => {
        post.created_at = moment(post.created_at).format('YYYY/MM/DD HH:mm:ss')
        return post
      })
    },
    ...mapGetters('posts', ['posts'])
  },
  methods: {
    handleClick(post) {
      this.$router.push(`/posts/${post.id}`)
    }
  }
}
</script>

<style>
.post-page .el-table__row {
  cursor: pointer;
}

</style>