<template>
  <v-app>
    <v-container>
      <edit />
    </v-container>
  </v-app>
</template>

<script>
import edit from "@/components/EditUser";
export default {
  layout: "administrador",
  components: {
    edit,
  },
  beforeMount() {},
  data() {
    return {};
  },
  methods: {},
};
</script>

<style>
</style>