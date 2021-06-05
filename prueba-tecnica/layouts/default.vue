<template>
  <v-app>
    <v-main>
      <v-container>
        <v-layout row class="text-xs-center">
          <v-flex xs4 sm6>
            <v-container
              style="position: relative; left: 50%"
              class="justify-center"
            >
            </v-container>
          </v-flex>
        </v-layout>
      </v-container>
      <v-footer app>
        <v-spacer></v-spacer>
        <span>Vuetify &copy; {{ new Date().getFullYear() }}</span>
        <v-spacer></v-spacer>
      </v-footer>
      <nuxt />
    </v-main>
  </v-app>
</template>

<script>
export default {};
</script>

<style>
#app {
  background: url("../static/images/FondoIndex.png") no-repeat center fixed !important;
  background-size: cover;
}
</style>