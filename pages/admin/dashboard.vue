<template>
  <v-main class="pa-0">
    <v-row>
      <v-col cols="12" md="3">
        <v-card
          to="/admin/categories"
          class="pa-5 my-3"
          style="max-width: 18rem; background-image: radial-gradient(780.6px at 10% 20%, rgb(133 238 255) 0%, rgb(255 125 125) 90.7%);"
        >
          <v-card-title class="justify-center mb-3"><v-icon large>mdi-shape</v-icon></v-card-title>
          <v-card-subtitle class="text-center">
            التصنيفات
            <strong>(7)</strong>
          </v-card-subtitle>
        </v-card>
      </v-col>
      <v-col cols="12" md="3">
        <v-card
          to="/admin/users"
          class="pa-5 my-3"
          style="max-width: 18rem; background-image: radial-gradient( circle 780.6px at 10% 20%,  rgba(133,255,189,1) 0%, rgb(255 125 125) 90.7%);"
        >
          <v-card-title class="justify-center mb-3"><v-icon large>mdi-account-group</v-icon></v-card-title>
          <v-card-subtitle class="text-center">
            المستخدمين
            <strong>(50)</strong>
          </v-card-subtitle>
        </v-card>
      </v-col>
      <v-col cols="12" md="3">
        <v-card
          to="/admin/roles"
          class="pa-5 my-3"
          style="max-width: 18rem;     background-image: radial-gradient(780.6px at 10% 20%, rgb(255 212 133) 0%, rgb(255 125 125) 90.7%);"
        >
          <v-card-title class="justify-center mb-3"><v-icon large>mdi-account-star</v-icon></v-card-title>
          <v-card-subtitle class="text-center">
            الصلاحيات
            <strong>(3)</strong>
          </v-card-subtitle>
        </v-card>
      </v-col>
    </v-row>
    <v-snackbar
      v-model="message"
      :timeout="3000"
      elevation="0"
      color="transparent"
    >
      <v-alert
        dismissible
        elevation="3"
        border="left"
        type="success"
        width="300"
      >
        تم تعديل المعلومات بنجاح..
      </v-alert>
    </v-snackbar>
  </v-main>
</template>

<script>
export default {
  data () {
    return {
      message: false
    }
  },
  head () {
    return {
      title: 'لوحة التحكم'
    };
  },
  computed: {
    getUser () {
      return this.$store.getters.getUserInfo.userName;  // it get current user
    },
  },
  methods: {

  },
}
</script>
