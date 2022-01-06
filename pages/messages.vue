<template>
  <v-card max-width="100%" class="mx-auto">
    <v-toolbar color="#6FBBAC" dark>
      <v-menu bottom offset-y left>
        <template v-slot:activator="{ on, attrs }">
          <v-btn icon v-bind="attrs" v-on="on">
            <v-icon>mdi-menu</v-icon>
          </v-btn>
        </template>
        <v-list>
          <v-list-item v-for="(item, i) in itemsMenu" :key="i" link>
            <v-list-item-title @click="selectTitle(i)">
              {{ item.title }}
            </v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>

      <v-toolbar-title class="selectedTitle">{{
        selectedTitle
      }}</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>
    </v-toolbar>
    <div v-if="numberSelectedTitle == 0">
      <inbox-messages></inbox-messages>
    </div>
    <div v-if="numberSelectedTitle == 1">
      <UsersSendMessage></UsersSendMessage>
    </div>
  </v-card>
</template>

<style >
.selectedTitle {
  padding-top: 3px;
}
</style>

<script>
export default {
  data() {
    return {
      selectedTitle: null,
      numberSelectedTitle: Number,
      itemsMenu: [
        {
          title: 'رسائلي',
        },
        {
          title: 'ارسال رسالة جديدة',
        },
      ],
    }
  },
  head() {
    this.selectTitle(0)
  },
  methods: {
    selectTitle(itemId) {
      this.numberSelectedTitle = itemId
      if (itemId === 0) this.selectedTitle = 'رسائلي'
      else if (itemId === 1) this.selectedTitle = 'ارسال رسالة جديدة'
    },
  },
}
</script>