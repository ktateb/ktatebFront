<template>
  <v-parallax src="/register.jpg">
    <v-overlay overlay>
      <v-container>
        <v-main>
          <v-card class="pa-5" light>
            <v-form
              ref="signup"
              lazy-validation
              @submit.prevent="signup()"
            >
              <router-link to="/">
                <v-img
                  class="ma-auto"
                  alt="كتاتيب"
                  max-height="100%"
                  max-width="120"
                  src="logo.png"
                ></v-img>
              </router-link>
              <v-card-title class="justify-center mb-1">أهلاً بك في منصة كتاتيب التعليميَّة, سجِّل الآن</v-card-title>
              <v-card-subtitle class="text-center">
                لديك حساب بالفعل؟
                <router-link to="/login">
                  تسجيل الدخول
                </router-link>
              </v-card-subtitle>
              <v-row>
                <v-col cols="12" sm="6" class="pt-0">
                  <v-text-field
                    v-model="newUser.firstName"
                    label="الاسم الأوَّل"
                    prepend-inner-icon="mdi-account"
                    class="ma-0"
                    :rules="[rules.required]"
                  ></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" class="pt-0">
                  <v-text-field
                    v-model="newUser.lastName"
                    label="الاسم الأخير"
                    prepend-inner-icon="mdi-account"
                    class="ma-0"
                    :rules="[rules.required]"
                  ></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" class="pt-0">
                  <v-text-field
                    v-model="newUser.email"
                    label="البريد الإلكتروني"
                    prepend-inner-icon="mdi-email"
                    class="ma-0"
                    :rules="[rules.required, rules.email]"
                  ></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" class="pt-0">
                  <v-text-field
                    v-model="newUser.phoneNumber"
                    label="رقم الهاتف"
                    prepend-inner-icon="mdi-cellphone"
                    class="ma-0"
                    :rules="[rules.required]"
                  ></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" class="pt-0">
                  <v-text-field
                    v-model="newUser.userName"
                    label="اسم المستخدم"
                    prepend-inner-icon="mdi-account-circle"
                    class="ma-0"
                    :rules="[rules.required]"
                  ></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" class="pt-0">
                  <v-text-field
                    v-model="newUser.password"
                    :append-icon="showPass ? 'mdi-eye' : 'mdi-eye-off'"
                    prepend-inner-icon="mdi-lock"
                    :rules="[rules.required, rules.password[0], rules.password[1], rules.password[2], rules.password[3], rules.password[4]]"
                    :type="showPass ? 'text' : 'password'"
                    label="كلمة المرور"
                    class="input-group--focused ma-0"
                    @click:append="showPass = !showPass"
                  ></v-text-field>
                </v-col>
                <v-col cols="12" sm="6" class="pt-0">
                  <v-menu
                    ref="menu"
                    v-model="menu"
                    :close-on-content-click="false"
                    transition="scale-transition"
                    offset-y
                    min-width="auto"
                    class="ma-0"
                  >
                    <template v-slot:activator="{ on, attrs }">
                      <v-text-field
                        v-model="newUser.birthday"
                        label="تاريخ الميلاد"
                        prepend-inner-icon="mdi-calendar"
                        :rules="[rules.required]"
                        readonly
                        v-bind="attrs"
                        class="ma-0"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <v-date-picker
                      v-model="newUser.birthday"
                      :active-picker.sync="activePicker"
                      :max="(new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10)"
                      min="1950-01-01"
                      @change="saveDate"
                    ></v-date-picker>
                  </v-menu>
                </v-col>
                <v-col cols="12" sm="6" class="pt-0">
                  <v-autocomplete
                    v-model="newUser.country"
                    :items="countries"
                    label="اختر الدولة"
                    prepend-inner-icon="mdi-map-marker"
                    :rules="[rules.required]"
                    no-data-text="لا يوجد دولة مطابقة!"
                    class="ma-0"
                  ></v-autocomplete>
                </v-col>
                <v-col cols="12" sm="4" class="pt-0">
                  <div class="mb-2">
                    <v-icon>mdi-gender-male-female</v-icon>
                    <span>حدد الجنس</span>
                  </div>
                  <v-radio-group
                    v-model="newUser.gender"
                    :rules="[rules.required]"
                    row
                    class="ma-0"
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
                </v-col>
                <v-col cols="12" sm="8" class="pt-0">
                  <small>
                    بالضفط على "إنشاء الحساب" فأنت توافق على
                    <router-link to="/">
                      سياسة الخصوصية
                    </router-link>
                    و
                    <router-link to="/">
                      شروط الاستخدام
                    </router-link>
                  </small>
                </v-col>
              </v-row>
              <v-card-actions class="justify-center">
                <v-btn
                  color="primary"
                  elevation="2"
                  type="submit"
                  x-large
                  block
                >
                  <v-icon class="me-1">mdi-account-plus</v-icon>
                  إنشاء الحساب
                </v-btn>
              </v-card-actions>
            </v-form>
          </v-card>
        </v-main>
      </v-container>
    </v-overlay>
  </v-parallax>
</template>

<script>
export default {
  layout: 'empty',
  middleware: "loggedin",
  data () {
    return {
      newUser: {
        firstName: '',
        lastName: '',
        email: '',
        phoneNumber: '',
        userName: '',
        password: '',
        birthday: null,
        gender: Number,
        country: '',
      },
      showPass: false,
      activePicker: null,
      menu: false,
      countries: [],
      rules: {
        required: value => !!value || 'هذا الحقل مطللوب ..!',
        password: [
          v => /(?=.*[a-z])/.test(v) || 'يجب أن تحتوي على محرف واحد صغير على الأقل',
          v => /(?=.*[A-Z])/.test(v) || 'يجب أن تحتوي على محرف واحد كبير على الأقل',
          v => /(?=.*\d)/.test(v) || 'يجب أن تحتوي على رقم واحد على الأقل',
          v => /([!@$*%])/.test(v) || 'يجب أن تحتوي على أحد الرموز التالية على الأقل [! @ # $ * %]',
          v => (v && v.length >= 8) || 'يجب أن تتكون من 8 خانات على الأقل'
        ],
        email: value => {
          const pattern = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
          return pattern.test(value) || 'الرجاء عنوان بريد إلكتروني صحيح ..!'
        },
      },
    }
  },
  head() {
    return {
      title: "إنشاء حساب"
    };
  },
  computed: {
    form () {
      return {
        country: this.newUser.country,
      }
    },
  },
  watch: {
    menu (val) {
      val && setTimeout(() => (this.activePicker = 'YEAR'))
    },
  },
  mounted () {
    this.getCountries()
  },
  methods: {
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
    saveDate (date) {
      this.$refs.menu.save(date)
    },
    async signup() {
      if (this.$refs.signup.validate()) {
        try {
          await this.$axios.$post('/api/Account/Register', this.newUser);
          const user = {
            userNameOrEmail: this.newUser.userName,
            password: this.newUser.password
          }
          await this.$auth.loginWith("local", {
            data: user
          });
          this.$router.push("/");
        } catch (err) {
          console.log(err);
        }
      }
    },
    // signup () {
    //   if (this.$refs.signup.validate()) {
    //     this.$axios.$post('/api/Account/Register', this.newUser)
    //       .then(() => {
    //         this.$refs.signup.reset()
    //         console.log("USER INSERTED..")
    //       })
    //       .catch((err) => {
    //         console.log(err)
    //       })
    //   }
    // },
  },
}
</script>

<style lang="scss">
  .v-parallax {
    height: 100vh!important;
    .v-overlay__content {
      width: 100%;
      .v-card {
        background-color: rgba(255, 255, 255, .9);
        width: 30rem;
        margin: auto;
      }
    }
  }
</style>
