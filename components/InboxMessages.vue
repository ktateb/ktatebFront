<template>
  <div class="bodyCard">
    <v-list three-line>
      <template v-for="(item, index) in items">
        <v-subheader
          v-if="item.header"
          :key="item.header"
          v-text="item.header"
        ></v-subheader>

        <v-divider
          v-else-if="item.divider"
          :key="index"
          :inset="item.inset"
        ></v-divider>

        <v-list-item v-else :key="item.title" link to="/chat">
          <v-list-item-avatar height="55" width="55">
            <v-img :src="item.avatar"></v-img>
          </v-list-item-avatar>

          <v-list-item-content>
            <v-list-item-userName
              v-html="item.userName"
              class="userName"
            ></v-list-item-userName>
            <v-list-item-message
              v-html="item.message"
              class="sentMessage"
            ></v-list-item-message>
            <v-list-item-dateSent
              v-html="item.dateSent"
              class="dateSent"
            ></v-list-item-dateSent>
          </v-list-item-content>
        </v-list-item>
      </template>
    </v-list>
  </div>
</template>

<style>
.dateSent {
  font-size: 10px;
}
.sentMessage {
  font-size: 14px;
}
.userName {
  color: #6fbbac;
  font-weight: bold;
  padding-top: 7px;
}
.bodyCard {
  overflow-y: scroll;
  height: 480px;
  border: 0 solid;
}
</style>

<script>
export default {
  data: () => ({
    items: [
      // { header: 'Today' },
      {
        avatar: 'https://cdn.vuetifyjs.com/images/lists/1.jpg',
        userName: 'Sarya Tulimat',
        message: 'هييي يا صديقي',
        dateSent: '22/12/2009',
      },
      { divider: true, inset: true },
      {
        avatar: 'https://cdn.vuetifyjs.com/images/lists/2.jpg',
        userName: 'Hussam Bashat',
        message: 'عاااش يا حسام ال vue خارقة ',
        dateSent: '22/12/2009',
      },
      { divider: true, inset: true },
      {
        avatar: 'https://cdn.vuetifyjs.com/images/lists/3.jpg',
        userName: 'Mohammad Zehrawi',
        message: 'احلا شباااب والله',
        dateSent: '22/12/2009',
      },
      { divider: true, inset: true },
      {
        avatar: 'https://cdn.vuetifyjs.com/images/lists/4.jpg',
        userName: 'Ahamd nour',
        message: 'شو الاخبار؟',
        dateSent: '22/12/2009',
      },
      { divider: true, inset: true },
      {
        avatar: 'https://cdn.vuetifyjs.com/images/lists/5.jpg',
        userName: 'Ahamd nour copy version',
        message: 'مرحبا انا احمد نور و عجبتني هل فيو',
        dateSent: '22/12/2009',
      },
    ],
  }),
}
</script>