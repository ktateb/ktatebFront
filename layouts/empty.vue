<template>
  <v-app>
    <Nuxt />
  </v-app>
</template>

<script>
export default {
  name: 'Empty',
  mounted() {
    const theme = localStorage.getItem("themeDarkMode");
      if (theme) {
        if (theme === "true") {
          this.$vuetify.theme.dark = true;
        } else {
          this.$vuetify.theme.dark = false;
        }
      }
  },
}
</script>
