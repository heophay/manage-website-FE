<template>
  <div>
    <div>
      {{ userid }}
      <nuxt-link class="button" :to="{ name: 'index' }">Back</nuxt-link>
    </div>
  </div>
</template>

<script>
export default {
  name:'UserPage',
  data() {
    return {
      userid: this.$route.params.userid
    }
  }
}
</script>

<style lang="scss" modules>
.userid-page {
  background-color: red;
}
</style>
