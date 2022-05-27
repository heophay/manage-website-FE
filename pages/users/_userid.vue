<template>
  <div>
    <div>
      {{ userid }}
      <nuxt-link class="button" :to="{ name: 'index' }">Back</nuxt-link>
      <div class="button" @click="click">click de</div>
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
  },
  methods: {
    click() {
      console.log(this)
    }
  }
}
</script>

<style lang="scss" modules>
.userid-page {
  background-color: red;
}
</style>
