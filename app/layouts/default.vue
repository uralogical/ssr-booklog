<template>
  <div>
    <Header />
    <div class='wrapper'>
      <nuxt class='container' />
    </div>
  </div>
</template>


<script>
import Header from '~/components/Header.vue'

export default {
  components: {
    Header
  }
}
</script>