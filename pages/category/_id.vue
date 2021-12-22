<template>
  <v-main>
    <v-card class="pa-5 my-3 mx-auto" style="width: 40rem">
      <v-card-title class="justify-center mb-3">
        Categories
      </v-card-title>
      <v-card-subtitle class="text-center">
        <h1> title </h1>
      </v-card-subtitle>
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
        تم تعديل الصورة الشخصية بنجاح..
      </v-alert>
    </v-snackbar>
  </v-main>
</template>

<script>
export default {
  layout: 'stretch',
  data () {
    return {
      courses: [],
      message: false,
    }
  },
  head () {
    return {
      title: 'التصنيفات'
    };
  },
  mounted () {
    this.getCourses()
  },
  methods: {
    getCourses () {
      this.$axios.$post(`/api/Category/${parseInt(this.$route.params.id)}/Courses`, { Id: parseInt(this.$route.params.id) })
        .then((res) => {
          this.courses = res
        })
        .catch((err) => {
          console.log(err)
        })
    },
  },
}
</script>
