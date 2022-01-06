<template>
  <v-card max-width="100%" class="mx-auto">
    <v-toolbar color="#6FBBAC" dark>
      <v-img
        max-height="100%"
        max-width="40"
        class="chatImage"
        src="/user.png"
      ></v-img>
      <v-toolbar-title class="nameUser">
        <nuxt-link to="#" style="color: white"> أحمد نور </nuxt-link>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>
    </v-toolbar>
    <memberMessages></memberMessages>
  </v-card>
</template>

<style>
.nameUser {
  padding-top: 5px;
  padding-right: 10px;
}
</style>