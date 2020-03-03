<template>
  <el-menu mode='horizontal' :router='true' class='header'>
    <el-menu-item class="menuItem" index='1' style='pointer-events:none;'>
      BookLog
    </el-menu-item>
    <el-menu-item class="menuItem" index='2' :route="{ path: '/posts' }">
      本の一覧
    </el-menu-item>

    <no-ssr>
      <el-menu-item
        v-if='user'
        index='4'
        style='float: right;'
        class="menuItem"
        :route="{ path: `/users/${user.id}` }">
        <span>{{ user.id }}</span>
      </el-menu-item>
      <el-menu-item
        v-else
        index='4'
        style='float: right;'
        class="menuItem"
        :route="{ path: '/' }">
        <span>ログイン</span>
      </el-menu-item>
      <el-menu-item
        index='5'
        style='float: right;'
        class="menuItem"
        :route="{ path: '/posts/new' }">
        本を登録
      </el-menu-item>
    </no-ssr>
  </el-menu>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  computed: {
    ...mapGetters(['user'])
  }
}
</script>
<style scoped>
.header {
  background-color: #003f5c;
}

.menuItem {
  color: #fff;
}
</style>
