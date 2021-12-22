<template>
  <v-parallax src="/register.jpg">
    <v-overlay overlay>
      <v-container>
        <v-main>
          <v-card class="pa-5" light>
            <v-form
              ref="login"
              lazy-validation
              @submit.prevent="login()"
            >
              <nuxt-link to="/">
                <v-img
                  class="ma-auto"
                  alt="كتاتيب"
                  max-height="100%"
                  max-width="120"
                  src="logo.png"
                ></v-img>
              </nuxt-link>
              <v-card-title class="justify-center mb-1">مرحباً بعودتك, قم بتسجيل الدخول</v-card-title>
              <v-card-subtitle class="text-center">
                ليس لديك حساب بعد؟
                <nuxt-link to="/signup">
                  إنشاء حساب
                </nuxt-link>
              </v-card-subtitle>
              <v-alert
                v-if="errorLogin"
                border="right"
                colored-border
                type="error"
                elevation="2"
              >
                هناك خطأ في اسم المستخدم أو كلمة المرور ..!
              </v-alert>
              <v-text-field
                v-model="user"
                label="اسم المستخدم او البريد الإلكتروني"
                prepend-inner-icon="mdi-account-circle"
                :rules="[rules.required]"
              ></v-text-field>
              <v-text-field
                v-model="password"
                :append-icon="showPass ? 'mdi-eye' : 'mdi-eye-off'"
                prepend-inner-icon="mdi-lock"
                :rules="[rules.required]"
                :type="showPass ? 'text' : 'password'"
                label="كلمة المرور"
                class="input-group--focused"
                @click:append="showPass = !showPass"
              ></v-text-field>
              <div class="d-flex align-center justify-space-between">
                <v-checkbox
                  v-model="remember"
                  label="تذكرني على هذا الجهاز"
                ></v-checkbox>
                <v-dialog
                  v-model="dialog"
                  persistent
                  max-width="600px"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn
                      text
                      v-bind="attrs"
                      v-on="on"
                    >
                      نسيت كلمة المرور؟
                    </v-btn>
                  </template>
                  <v-card class="py-3">
                    <v-card-title>
                      لا تقلق سنعمل على إعادة تعيين كلمة المرور الخاصة بك بخطوات قليلة ..
                    </v-card-title>
                    <v-card-text>
                      <v-text-field
                        v-if="!emailExist && dialog == true"
                        ref="mail"
                        v-model="email"
                        label="ادخل البريد الإلكتروني الذي قمت بالتسجيل من خلاله"
                        prepend-inner-icon="mdi-email"
                        :rules="[rules.required, rules.email]"
                      ></v-text-field>
                      <v-text-field
                        v-else
                        v-model="newPass"
                        :append-icon="showPass ? 'mdi-eye' : 'mdi-eye-off'"
                        prepend-inner-icon="mdi-lock"
                        :rules="[rules.required, rules.password[0], rules.password[1], rules.password[2], rules.password[3], rules.password[4]]"
                        :type="showPass ? 'text' : 'password'"
                        label="أدخل كلمة المرور الجديدة"
                        class="input-group--focused"
                        @click:append="showPass = !showPass"
                      ></v-text-field>
                    </v-card-text>
                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn
                        color="blue darken-1"
                        text
                        @click="dialog = false"
                      >
                        إلغاء الأمر
                      </v-btn>
                      <v-btn
                        v-if="!emailExist"
                        color="primary"
                        @click="checkEmailToResetPass()"
                      >
                        التالي
                      </v-btn>
                      <v-btn
                        v-else
                        color="primary"
                        @click="submitPassword()"
                      >
                        حفظ
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
              </div>
              <v-card-actions class="justify-center">
                <v-btn
                  color="primary"
                  elevation="2"
                  type="submit"
                  x-large
                  block
                >
                  <v-icon>mdi-login</v-icon>
                  دخول
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
      showPass: false,
      user: '',
      password: '',
      remember: false,
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
      dialog: false,
      email: '',
      emailExist: false,
      newPass: '',
      errorLogin: false
    }
  },
  head() {
    return {
      title: "تسجيل الدخول"
    };
  },
  methods: {
    checkEmailToResetPass () {
      if (this.$refs.mail.validate()) {
        this.emailExist = true
      }
    },
    async login () {
      if (this.$refs.login.validate()) {
        try {
          await this.$auth.loginWith("local", {
            data: {
              userNameOrEmail: this.user,
              password: this.password
            }
          });
          this.$router.push("/");
        } catch (err) {
          this.errorLogin = true
          setTimeout(() => {
            this.errorLogin = false
          }, 3000);
        }
      }
    },
    // login () {
    //   if (this.$refs.login.validate()) {
    //     this.$axios.$post('/api/Account/Login', {
    //       userNameOrEmail: this.user,
    //       password: this.password
    //     })
    //       .then(() => {
    //         this.$refs.login.reset()
    //         console.log("USER Logedin..")
    //       })
    //       .catch((err) => {
    //         console.log(err)
    //       })
    //   } else {
    //     console.log("INVALID..!")
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
