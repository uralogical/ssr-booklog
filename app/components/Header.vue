<template>
  <el-menu mode='horizontal' :router='true'>
    <el-menu-item index='1' style='pointer-events:none;'>
      SSR BookLog
    </el-menu-item>
    <el-menu-item index='2' :route="{ path: '/posts' }">
      本の一覧
    </el-menu-item>

    <no-ssr>
      <el-menu-item
        v-if='user'
        index='4'
        style='float: right;'
        :route="{ path: `/user/${user.id}` }">
        <span>{{ user.id }}</span>
      </el-menu-item>
      <el-menu-item
        v-else
        index='4'
        style='float: right;'
        :route="{ path: '/' }">
        <span>ログイン</span>
      </el-menu-item>
      <el-menu-item
        index='5'
        style='float: right;'
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