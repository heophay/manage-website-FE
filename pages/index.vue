<template>
  <div>
    <div :class="$style.heophay">huy ron </div>
    <nuxt-link
    class="button"
    :to="{name: 'users-userid', params: {userid: 1}}"
    >
      Get user ID
    </nuxt-link>
    <ListBills />
  </div>
</template>

<script>
// import axios from 'axios'
export default {
  name: 'IndexPage',
}
</script>

<style lang="scss" module>
.heophay {
  background-color: red;
  color: #fff;
}
</style>
