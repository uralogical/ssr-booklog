<template>
  <section class='container'>
    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <div class="accountBox">
          <el-row>
            <el-avatar :size="50" :src="userImageUrl"></el-avatar>
            <span>@{{ user.id }}</span>
          </el-row>
        </div>
        <el-button style="float: right; padding: 3px 0" type="text">Edit</el-button>
      </div>
      <div v-for="o in 4" :key="o" class="text item">
        
      </div>
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
  created() {
    console.log(this.user)
  },
  computed: {
    userImageUrl() {
      return 'https://qiita-user-profile-images.imgix.net/https%3A%2F%2Fqiita-image-store.s3.amazonaws.com%2F0%2F79598%2Fprofile-images%2F1518151304?ixlib=rb-1.2.2&auto=compress%2Cformat&lossless=0&w=300&s=73dec371662509c68d78ac4b918bb2f4'
    },
    ...mapGetters(['user'])
  },
  methods: {
    handleClick(post) {
      this.$router.push(`/posts/${post.id}`)
    }
  }
}
</script>

<style>
.accountBox {
  display:flex;
  justify-content: center;
}
.text {
  font-size: 14px;
}

.item {
  margin-bottom: 18px;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}
.clearfix:after {
  clear: both
}

.box-card {
  width: 480px;
}
</style>