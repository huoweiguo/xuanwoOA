<template>
  <router-view v-if="!$route.meta.link" :key="key" />
</template>

<script>
export default {
  computed: {
    key() {
      return this.$route.path
    }
  }
}
</script>

<style>

</style>