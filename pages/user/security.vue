<template>
  <v-main class="pa-0">
    <v-card class="pa-5 my-3 mx-auto" style="width: 40rem">
      <v-card-title class="justify-center mb-3"><h1> الحماية </h1></v-card-title>
      <v-card-subtitle class="text-center">
        يمكنك من هنا تعديل كلمة المرور
      </v-card-subtitle>
      <v-form
        ref="update"
        lazy-validation
        @submit.prevent="updatePassword()"
      >
        <v-text-field
          v-model="udpateUser.currentPassword"
          :append-icon="showPass1 ? 'mdi-eye' : 'mdi-eye-off'"
          prepend-inner-icon="mdi-lock"
          :rules="[rules.required]"
          :type="showPass1 ? 'text' : 'password'"
          label="كلمة المرور الحاليَّة"
          class="input-group--focused"
          @click:append="showPass1 = !showPass1"
        ></v-text-field>
        <small
          v-if="error"
          class="error--text"
        >
          <v-icon class="error--text">mdi-exclamation</v-icon>
          كلمة المرور الحاليَّة غير صحيحة!
        </small>
        <v-text-field
          v-model="udpateUser.password"
          :append-icon="showPass2 ? 'mdi-eye' : 'mdi-eye-off'"
          prepend-inner-icon="mdi-lock"
          :rules="[rules.required, rules.password[0], rules.password[1], rules.password[2], rules.password[3], rules.password[4]]"
          :type="showPass2 ? 'text' : 'password'"
          label="كلمة المرور الجديدة"
          class="input-group--focused"
          @click:append="showPass2 = !showPass2"
        ></v-text-field>
        <v-card-actions class="justify-center">
          <v-btn
            color="primary"
            elevation="2"
            type="submit"
            x-large
            class="px-4"
          >
            <v-icon class="me-1">mdi-check</v-icon>
            حفظ
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
        userName: this.getUser,
        currentPassword: '',
        password: '',
      },
      showPass1: false,
      showPass2: false,
      error: false,
      rules: {
        required: value => !!value || 'هذا الحقل مطللوب ..!',
        password: [
          v => /(?=.*[a-z])/.test(v) || 'يجب أن تحتوي على محرف واحد صغير على الأقل',
          v => /(?=.*[A-Z])/.test(v) || 'يجب أن تحتوي على محرف واحد كبير على الأقل',
          v => /(?=.*\d)/.test(v) || 'يجب أن تحتوي على رقم واحد على الأقل',
          v => /([!@$*%])/.test(v) || 'يجب أن تحتوي على أحد الرموز التالية على الأقل [! @ # $ * %]',
          v => (v && v.length >= 8) || 'يجب أن تتكون من 8 خانات على الأقل'
        ],
      },
      message: false
    }
  },
  head () {
    return {
      title: 'الحماية'
    };
  },
  computed: {
    getUser () {
      return this.$store.getters.getUserInfo.userName;  // it get current user
    },
  },
  methods: {
    updatePassword () {
      if (this.$refs.update.validate()) {
        this.$axios.$post('/api/Account/ChangePassword', this.udpateUser)
          .then(() => {
            this.message = true
            this.error = false
            this.$refs.update.reset()
          })
          .catch((err) => {
            console.log(err)
            this.error = true
          })
      }
    },
  },
}
</script>
