<template>
  <v-main class="pa-0">
    <v-card class="pa-5 my-3 mx-auto" style="width: 40rem">
      <v-card-title class="justify-center mb-3">
        <v-avatar size="100" class="user-avatar">
          <v-img :src="user.pictureUrl != null ? user.pictureUrl : '/user.png'"></v-img>
          <v-icon
            v-if="$route.params.username == user.userName"
           @click="dialog = true"
          >mdi-camera</v-icon>
        </v-avatar>
      </v-card-title>
      <v-card-subtitle class="text-center">
        <h1> {{ user.displayName }} </h1>
      </v-card-subtitle>
      <v-card-subtitle class="text-center">
        <v-tooltip
          v-for="(badge, k) in badges"
          :key="k"
          bottom>
          <template v-slot:activator="{ on, attrs }">
            <v-chip
              class="mx-1"
              v-bind="attrs"
              v-on="on"
            >
              <v-avatar>
                <v-icon>{{ badge.icon }}</v-icon>
              </v-avatar>
            </v-chip>
          </template>
          <span>{{ badge.tip }}</span>
        </v-tooltip>
      </v-card-subtitle>
      <v-divider class="my-3"></v-divider>
      <v-list two-line>
        <v-list-item>
          <v-list-item-icon>
            <v-icon>
              mdi-cellphone
            </v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ user.phoneNumber }}</v-list-item-title>
            <v-list-item-subtitle>رقم الهاتف</v-list-item-subtitle>
          </v-list-item-content>
          <v-list-item-icon class="ma-2">
            <v-btn icon>
              <a :href="`tel:${user.phoneNumber}`">
                <v-icon>mdi-phone</v-icon>
              </a>
            </v-btn>
          </v-list-item-icon>
        </v-list-item>

        <v-list-item>
          <v-list-item-icon>
            <v-icon>
              mdi-email
            </v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ user.email }}</v-list-item-title>
            <v-list-item-subtitle>البريد الإلكتروني</v-list-item-subtitle>
          </v-list-item-content>
          <v-list-item-icon class="ma-2">
            <v-btn icon>
              <a :href="`mailto:${user.email}`">
                <v-icon>mdi-message-text</v-icon>
              </a>
            </v-btn>
          </v-list-item-icon>
        </v-list-item>

        <v-list-item>
          <v-list-item-icon>
            <v-icon>
              mdi-map-marker
            </v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ user.country }}</v-list-item-title>
            <v-list-item-subtitle>الدولة</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>

        <v-divider inset></v-divider>

        <v-list-item>
          <v-list-item-icon>
            <v-icon>
              mdi-gender-male-female
            </v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ user.gender == 'Male' ? 'ذكر' : 'انثى' }}</v-list-item-title>
            <v-list-item-subtitle>الجنس</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>

        <v-list-item>
          <v-list-item-icon>
            <v-icon>
              mdi-calendar
            </v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ new Date(user.birhtday).toLocaleDateString() }}</v-list-item-title>
            <v-list-item-subtitle>تاريخ الميلاد</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>

      </v-list>
    </v-card>
    <v-row justify="space-around">
      <v-col cols="auto">
        <v-dialog
          v-model="dialog"
          transition="dialog-bottom-transition"
          max-width="600"
          persistent
        >
          <v-form
            ref="update"
            lazy-validation
            @submit.prevent="updateAvatar()"
          >
            <v-card class="pb-3">
              <v-toolbar
                color="primary"
                dark
              >تغيير الصورة الشخصية</v-toolbar>
              <v-card-text>
                <v-file-input
                  v-model="avatar"
                  class="mt-5"
                  :rules="[rules.required, rules.fileSize]"
                  accept="image/png, image/jpeg, image/bmp"
                  placeholder="Pick an avatar"
                  prepend-inner-icon="mdi-camera"
                  label="اختر صورة"
                ></v-file-input>
              </v-card-text>
              <v-card-actions class="justify-end">
                <v-btn
                  text
                  @click="dialog = false"
                >إلغاء</v-btn>
                <v-btn
                  color="primary"
                  type="submit"
                >حفظ</v-btn>
              </v-card-actions>
            </v-card>
          </v-form>
        </v-dialog>
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
        تم تعديل الصورة الشخصية بنجاح..
      </v-alert>
    </v-snackbar>
  </v-main>
</template>

<script>
export default {
  data () {
    return {
      user: [],
      message: false,
      dialog: false,
      avatar: '',
      badges: [
        {
          type: 'Admin',
          icon: 'mdi-account-tie-hat',
          tip: 'مسؤول'
        },
        {
          type: 'Teacher',
          icon: 'mdi-account-tie',
          tip: 'مُدرِّس'
        },
        {
          type: 'Student',
          icon: 'mdi-school',
          tip: 'طالب'
        }
      ],
      rules: {
        required: value => !!value || 'هذا الحقل مطللوب ..!',
        fileSize: value => !value || value.size < 2000000 || 'حجم الصورة يجب أن يكون أقل من 2 MB!',
      },
    }
  },
  head () {
    return {
      title: this.name
    };
  },
  computed: {
    name () {
      return this.user.firstName
    }
  },
  mounted () {
    this.getUserData()
  },
  methods: {
    getUserData () {
      this.$axios.$get(`/api/Account/User/${this.$route.params.username}`)
        .then((res) => {
          this.user = res
          Array.from(this.badges).map(badge => !res.roles.includes(badge.type) ? this.badges.splice(this.badges.indexOf(badge), 1) : '')
          console.log(this.user)
          console.log(this.badges)
        })
        .catch((err) => {
          console.log(err)
        })
    },
    updateAvatar () {
      if (this.$refs.update.validate()) {
        const formData = new FormData();
        formData.append('PictureUrl', this.avatar);
        this.$axios.$post('/api/Account/UploadImage', formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        })
          .then(() => {
            this.message = true
            this.dialog = false
            this.getUserData()
          })
          .catch((err) => {
            console.log(err)
          })
      }
    },
  },
}
</script>

<style scoped lang="scss">
  .user-avatar {
    position: relative;
    &:hover .v-icon {bottom: 0!important;}
    .v-icon {
      position: absolute;
      width: 100%;
      background-color: #ccc;
      height: 40%;
      border-radius: 0;
      bottom: -100%;
      cursor: pointer;
    }
  }
</style>
