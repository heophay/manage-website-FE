<template>
  <div>
    <div>
      {{ billid }}
      <nuxt-link class="button" :to="{ name: 'index' }">Back</nuxt-link>
    </div>
  </div>
</template>

<script>
export default {
  name:'UserPage',
  layout: 'MainLayout',
  data() {
    return {
      billid: this.$route.params.billid
    }
  }
}
</script>

<style lang="scss" modules>
.userid-page {
  background-color: red;
}
</style>
