<template>
  <v-app>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="true"
      :right="true"
      fixed
      app
    >
      <v-list>
        <template
          v-for="(item, i) in items"
        >
          <v-list-item
            v-if="getUser.includes(item.rules) || item.rules === 'all'"
            :key="i"
            :to="item.to"
            router
            exact
          >
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title v-text="item.title" />
            </v-list-item-content>
          </v-list-item>
        </template>
      </v-list>
      <template v-slot:append>
        <v-list-item @click="logout">
          <v-list-item-action>
            <v-icon>mdi-logout</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            تسجيل الخروج
          </v-list-item-content>
        </v-list-item>
      </template>
    </v-navigation-drawer>
    <v-app-bar
      :clipped-right="true"
      fixed
      app
    >
      <nuxt-link to="/" class="me-8">
        <v-img
          alt="كتاتيب"
          max-height="100%"
          max-width="120"
          src="/logo.png"
        ></v-img>
      </nuxt-link>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer">
        <v-icon v-if="drawer">mdi-close</v-icon>
        <v-icon v-else>mdi-menu</v-icon>
      </v-app-bar-nav-icon>
      <v-btn
        icon
        @click.stop="miniVariant = !miniVariant"
      >
        <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
      </v-btn>
      <v-spacer></v-spacer>
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
            <v-icon>mdi-bell</v-icon>
          </v-btn>
        </template>
        <v-list>
          <v-list-item>
            <v-list-item-title>Notification One</v-list-item-title>
          </v-list-item>
          <v-list-item>
            <v-list-item-title>Notification Two</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
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
            <v-icon>mdi-message</v-icon>
          </v-btn>
        </template>
        <v-list>
          <v-list-item>
            <v-list-item-title>Message One</v-list-item-title>
          </v-list-item>
          <v-list-item>
            <v-list-item-title>Message Two</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
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
            <v-icon>mdi-cog</v-icon>
          </v-btn>
        </template>
        <v-list>
          <v-list-item
            v-for="(item, i) in settings"
            :key="i"
            :to="item.to"
          >
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-title> {{ item.title }} </v-list-item-title>
          </v-list-item>
          <v-list-item>
            <v-list-item-title class="d-flex justify-center ">
              <v-list-item-action class="mx-0">
                <v-icon>mdi-white-balance-sunny</v-icon>
              </v-list-item-action>
              <v-list-item-action class="mx-2">
                <v-switch
                  v-model="themeDark"
                  :value="themeDark"
                  hide-details
                  color="primary"
                  @change="toggleTheme()"
                ></v-switch>
              </v-list-item-action>
              <v-list-item-action class="mx-0">
                <v-icon>mdi-weather-night</v-icon>
              </v-list-item-action>
            </v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer class="justify-center">
      <small>
        جميع الحقوق محفوظة
        <strong>كتاتيب</strong> {{ new Date().getFullYear() }} &copy;
      </small>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  middleware: "authenticated",
  data () {
    return {
      drawer: true,
      items: [
        {
          icon: 'mdi-view-dashboard',
          title: 'لوحة التحكم',
          to: '/admin/dashboard',
          rules: 'Admin'
        },
        {
          icon: 'mdi-card-account-details',
          title: 'حول',
          to: `/user/${this.$route.params.username}`,
          rules: 'all'
        },
        {
          icon: 'mdi-play-circle',
          title: 'الكورسات',
          to: '/courses',
          rules: 'all'
        },
      ],
      settings: [
        {
          icon: 'mdi-account-circle',
          title: 'الحساب',
          to: `/user/settings`
        },
        {
          icon: 'mdi-lock',
          title: 'الحماية',
          to: '/user/security'
        },
      ],
      miniVariant: false,
      themeDark: Boolean
    }
  },
  computed: {
    getUser () {
      return this.$store.getters.getUserInfo.roles;  // it get current user
    },
  },
  mounted () {
    const theme = localStorage.getItem("themeDarkMode");
    if (theme) {
      if (theme === "true") {
        this.$vuetify.theme.dark = true;
        this.themeDark = true;
      } else {
        this.$vuetify.theme.dark = false;
        this.themeDark = false;
      }
    }
  },
  methods: {
    toggleTheme () {
      this.$vuetify.theme.dark = !this.$vuetify.theme.dark;
      this.themeDark = !this.$vuetify.theme.dark;
      localStorage.setItem("themeDarkMode", this.$vuetify.theme.dark.toString())
    },
    async logout () {
      await this.$auth.logout();
    },
  }
}
</script>
