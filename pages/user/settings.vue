<template>
  <v-main class="pa-0">
    <v-card class="pa-5 my-3 mx-auto" style="width: 40rem">
      <v-card-title class="justify-center mb-3"><h1> الحساب </h1></v-card-title>
      <v-card-subtitle class="text-center">
        يمكنك من هنا تعديل معلوماتك الشخصية
      </v-card-subtitle>
      <v-form
        ref="update"
        lazy-validation
        @submit.prevent="updateInfo()"
      >
        <v-text-field
          v-model="udpateUser.firstName"
          label="الاسم الأوَّل"
          prepend-inner-icon="mdi-account"
          :rules="[rules.required]"
        ></v-text-field>
        <v-text-field
          v-model="udpateUser.lastName"
          label="الاسم الأخير"
          prepend-inner-icon="mdi-account"
          :rules="[rules.required]"
        ></v-text-field>
        <v-text-field
          v-model="udpateUser.phoneNumber"
          label="رقم الهاتف"
          prepend-inner-icon="mdi-cellphone"
          :rules="[rules.required]"
        ></v-text-field>
        <v-menu
          v-model="menu"
          :close-on-content-click="false"
          :nudge-right="40"
          transition="scale-transition"
          offset-y
          min-width="auto"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-text-field
              v-model="udpateUser.birthday"
              label="تاريخ الميلاد"
              prepend-inner-icon="mdi-calendar"
              readonly
              v-bind="attrs"
              v-on="on"
            ></v-text-field>
          </template>
          <v-date-picker
            v-model="udpateUser.birthday"
            @input="menu = false"
          ></v-date-picker>
        </v-menu>
        <v-autocomplete
          v-model="udpateUser.country"
          :items="countries"
          label="اختر الدولة"
          prepend-inner-icon="mdi-map-marker"
          :rules="[rules.required]"
          no-data-text="لا يوجد دولة مطابقة!"
        ></v-autocomplete>
        <div class="mb-2">
          <v-icon>mdi-gender-male-female</v-icon>
          <span>حدد الجنس</span>
        </div>
        <v-radio-group
          v-model="udpateUser.gender"
          :rules="[rules.required]"
          row
        >
          <v-radio
            label="ذكر"
            value="1"
            class="ma-0"
          ></v-radio>
          <v-radio
            label="انثى"
            value="2"
          ></v-radio>
        </v-radio-group>
        <v-card-actions class="justify-center">
          <v-btn
            color="primary"
            elevation="2"
            type="submit"
            x-large
            class="px-4"
          >
            <v-icon class="me-1">mdi-check</v-icon>
            حفظ المعلومات
          </v-btn>
        </v-card-actions>
      </v-form>
    </v-card>
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
      udpateUser: {
        id: '',
        firstName: '',
        lastName: '',
        phoneNumber: '',
        birthday: null,
        gender: '',
        country: '',
      },
      menu: false,
      countries: [],
      rules: {
        required: value => !!value || 'هذا الحقل مطللوب ..!',
      },
      message: false
    }
  },
  head () {
    return {
      title: 'الإعدادات'
    };
  },
  computed: {
    form () {
      return {
        country: this.udpateUser.country,
      }
    },
    getUser () {
      return this.$store.getters.getUserInfo.userName;  // it get current user
    },
  },
  mounted () {
    this.getCountries()
    this.getUserData(this.getUser)
  },
  methods: {
    getUserData (user) {
      this.$axios.$get(`/api/Account/User/${user}`)
        .then((res) => {
          console.log("DATE", res.birhtday)
          this.udpateUser.id = res.id
          this.udpateUser.firstName = res.firstName
          this.udpateUser.lastName = res.lastName
          this.udpateUser.phoneNumber = res.phoneNumber
          this.udpateUser.birthday = (new Date(Date.now() - (new Date(res.birhtday)).getTimezoneOffset() * 60000)).toISOString().substr(0, 10)
          this.udpateUser.country = res.country
          res.gender === 'Male' ? this.udpateUser.gender = "1" : this.udpateUser.gender = "2"
          console.log(this.udpateUser)
        })
        .catch((err) => {
          console.log(err)
        })
    },
    getCountries () {
      this.$axios.$get('/api/Country/GetCountries')
        .then((res) => {
          res.result.forEach(country => {
            this.countries.push(country.name)
          })
        })
        .catch((err) => {
          console.log(err)
        })
    },
    updateInfo() {
      if (this.$refs.update.validate()) {
        this.$axios.$post('/api/Account/Update', this.udpateUser)
          .then(() => {
            this.message = true
          })
          .catch((err) => {
            console.log(err)
          })
      }
    },
  },
}
</script>
