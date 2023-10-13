<template>
  <v-app dark> </v-app>
</template>

<script>
  export default {
    name: "EmptyLayout",
    layout: "empty",
    props: {
      error: {
        type: Object,
        default: null,
      },
    },
    data() {
      return {};
    },
  };
</script>

<style scoped>
  h1 {
    font-size: 20px;
  }
</style>
