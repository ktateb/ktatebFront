<template>
  <v-app>
    <v-app-bar
      fixed
      app
    >
      <v-container class="pa-0 fill-height">
        <v-toolbar-title>
          <router-link to="/">
            <v-img
              alt="كتاتيب"
              max-height="100%"
              max-width="120"
              src="logo.png"
            ></v-img>
          </router-link>
        </v-toolbar-title>

        <v-spacer></v-spacer>

        <template v-if="!isAuthenticated">
          <v-menu
            v-model="fastLogin"
            :close-on-content-click="false"
            :nudge-width="100"
            offset-y
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn-toggle
                dense
                background-color="primary"
              >
                <v-tooltip bottom>
                  <template v-slot:activator="{ on: tooltip }">
                    <v-btn
                      v-bind="attrs"
                      v-on="{ ...tooltip, ...on }"
                    >
                      <v-icon>mdi-chevron-down</v-icon>
                    </v-btn>
                  </template>
                  <span>ادخل بسرعة</span>
                </v-tooltip>

                <v-btn>
                  <nuxt-link to="/login">
                    تسجيل الدخول
                  </nuxt-link>
                </v-btn>
              </v-btn-toggle>
            </template>

            <v-card class="pa-3">
              <v-form
                ref="login"
                lazy-validation
                @submit.prevent="login()"
              >
                <v-card-title class="justify-center pb-0">سجل دخولك بسرعة !</v-card-title>
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
                <v-card-actions class="justify-center">
                  <v-btn
                    color="primary"
                    elevation="2"
                    block
                    type="submit"
                  >
                    دخول
                  </v-btn>
                </v-card-actions>
                <v-alert
                  v-if="errorLogin"
                  dense
                  text
                  type="error"
                  class="ma-2"
                >
                 وصول غير صالح..!
                </v-alert>
              </v-form>
            </v-card>
          </v-menu>
        </template>
        <template v-else>
          <v-menu
            bottom
            offset-y
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                icon
                v-bind="attrs"
                v-on="on"
              >
                <v-img
                  src="/user.png"
                  max-width="40"
                ></v-img>
              </v-btn>
            </template>
            <v-list>
              <v-list-item>
                <v-list-item-title>
                  <nuxt-link :to="'/user/' + getUser">الملف الشخصي</nuxt-link>
                </v-list-item-title>
              </v-list-item>
              <v-list-item @click="logout">
                تسحيل الخروج
              </v-list-item>
            </v-list>
          </v-menu>
        </template>

        <template v-if="!isAuthenticated">
          <v-btn
            color="basePrimary"
            class="mx-2 white--text"
            elevation="2"
            small
            fab
            @click="toggleTheme()"
          >
            <v-icon v-if="$vuetify.theme.dark">mdi-white-balance-sunny</v-icon>
            <v-icon v-else>mdi-weather-night</v-icon>
          </v-btn>
        </template>

      </v-container>
    </v-app-bar>
    <Nuxt />
    <v-footer
      class="justify-center"
    >
      <v-container>
        <v-card
          class="flex"
          color="transparent"
          flat
        >
          <v-row>
            <v-col cols="12" md="3">
              <h4 class="py-5 text--text">روابط هامة</h4>
              <v-list-item
                v-for="item in importantLink"
                :key="importantLink.indexOf(item)"
                class="pa-0"
                style="min-height: auto"
              >
                <v-list-item-content class="py-2">
                  <v-list-item-title>
                    <router-link :to="item.link">{{ item.title }}</router-link>
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-col>
            <v-col cols="12" md="3">
              <h4 class="py-5 text--text">التصنيفات</h4>
              <v-list-item
                v-for="item in catLink"
                :key="catLink.indexOf(item)"
                class="pa-0"
                style="min-height: auto"
              >
                <v-list-item-content class="py-2">
                  <v-list-item-title>
                    <router-link :to="item.link">{{ item.title }}</router-link>
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-col>
            <v-col cols="12" md="3">
              <h4 class="py-5 text--text">نحن هنا</h4>
              <v-list-item
                v-for="item in ktateebLink"
                :key="ktateebLink.indexOf(item)"
                class="pa-0"
                style="min-height: auto"
              >
                <v-list-item-content class="py-2">
                  <v-list-item-title>
                    <router-link :to="item.link">{{ item.title }}</router-link>
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-col>
            <v-col cols="12" md="3">
              <h4 class="py-5 text--text">تابعنا على وسائل التواصل الاجتماعي</h4>
              <v-btn
                v-for="icon in icons"
                :key="icon"
                class="me-4"
                icon
              >
                <v-icon size="24px">
                  {{ icon }}
                </v-icon>
              </v-btn>
            </v-col>
          </v-row>
          <v-divider class="my-5"></v-divider>
          <v-card-text class="py-2 text-center">
            جميع الحقوق محفوظة
            <strong>كتاتيب</strong> {{ new Date().getFullYear() }} &copy;
          </v-card-text>
        </v-card>
      </v-container>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'Stretch',
  data: () => ({
    fastLogin: false,
    showPass: false,
    user: '',
    password: '',
    errorLogin: false,
    rules: {
      required: value => !!value || 'هذا الحقل مطللوب ..!',
    },
    icons: [
      'mdi-facebook',
      'mdi-twitter',
      'mdi-instagram',
      'mdi-linkedin',
    ],
    importantLink: [
      {link: '/faq', title: 'الأسئلة الشائعة'},
      {link: '/policy', title: 'سياسة الخصوصية'},
      {link: '/terms', title: 'شروط الاستخدام'},
    ],
    catLink: [
      {link: '/base', title: 'الشهادة الإعدادية'},
      {link: '/secondary', title: 'الشهادة الثانوية'},
      {link: '/courses', title: 'كورسات منوعة'},
    ],
    ktateebLink: [
      {link: '/blog', title: 'المدونة'},
      {link: '/contact', title: 'تواصل معنا'},
    ],
  }),
  computed: {
    isAuthenticated () {
      return this.$store.getters.isAuthenticated;  // it check if user isAuthenticated
    },
    getUser () {
      return this.$store.getters.getUserInfo.userName;  // it get current user
    },
  },
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
  methods: {
    toggleTheme () {
      this.$vuetify.theme.dark = !this.$vuetify.theme.dark;
      localStorage.setItem("themeDarkMode", this.$vuetify.theme.dark.toString())
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
          this.$router.push(`/user/${this.getUser}`);
        } catch (err) {
          this.errorLogin = true
          setTimeout(() => {
            this.errorLogin = false
          }, 3000);
        }
      }
    },
    async logout () {
      await this.$auth.logout();  // this method will logout the user and make token to false on the local storage of the user browser
    },
  },
}
</script>
