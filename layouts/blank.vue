<template>
  <v-app>
    <v-main>
      <nuxt />
    </v-main>
  </v-app>
</template>

<script>
export default {
  beforeMount() {
    localStorage.clear();
  },
};
</script>

<style scoped>
#app {
  background: url("../static/images/FondoPages.png") no-repeat center fixed !important;
}
</style>