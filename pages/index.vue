<template>
  <div>
    <v-main>
      <v-carousel
        cycle
        height="200"
        hide-delimiter-background
        hide-delimiters
        :show-arrows="false"
        interval="1500"
      >
        <v-carousel-item
          v-for="(slide, i) in slides"
          :key="i"
        >
          <v-sheet
            :color="colors[i]"
            height="100%"
          >
            <v-row
              class="fill-height"
              align="center"
              justify="center"
            >
              <div class="text-h2">
                <p class="heading-font font-weight-medium ma-0 white--text">
                  {{ slide }}
                </p>
              </div>
            </v-row>
          </v-sheet>
        </v-carousel-item>
      </v-carousel>
      <v-container>
        <v-row justify="center" align="center" class="py-12">
          <v-col cols="12" md="6">
            <p class="header-text text--text">
              <em class="primary--text"><strong>كتاتيب</strong></em>
              هي المنصة الأولى التي توفر التعليم عن بعد في سوريا, وتدعم بشكل أساسي طلاب الشهادات في المرحلة الإعدادية والثانوية..
            </p>
            <template v-if="!isAuthenticated">
              <nuxt-link to="/signup">
                <v-btn
                  x-large
                  color="primary"
                  class="effect"
                >
                  انضم الآن مجاناً
                </v-btn>
              </nuxt-link>
            </template>
          </v-col>
          <v-divider
            vertical
          ></v-divider>
          <v-col cols="12" md="4" class="offset-md-2 overflow-hidden mx-auto">
            <v-img
              alt="صورة"
              max-height="100%"
              max-width="100%"
              src="online.svg"
              class="scale-animate"
            ></v-img>
          </v-col>
        </v-row>
      </v-container>
      <section>
        <v-sheet
          class="pt-10"
          color="light"
        >
          <v-container>
            <h2 class="text-center primary--text mt-5 mb-10">على ماذا ستحصل؟</h2>
            <v-row>
              <v-col cols="12" md="3">
                <v-card elevation="0" color="transparent" class="my-5">
                  <v-img
                    src="Features/task-list.png"
                    width="100"
                    class="mx-auto"
                  ></v-img>
                  <v-card-title class="justify-center text--text">
                    خطة دراسة منظمة
                  </v-card-title>
                </v-card>
              </v-col>
              <v-col cols="12" md="3">
                <v-card elevation="0" color="transparent" class="my-5">
                  <v-img
                    src="Features/audience.png"
                    width="100"
                    class="mx-auto"
                  ></v-img>
                  <v-card-title class="justify-center text--text">
                    مدرسين مختصين
                  </v-card-title>
                </v-card>
              </v-col>
              <v-col cols="12" md="3">
                <v-card elevation="0" color="transparent" class="my-5">
                  <v-img
                    src="Features/research.png"
                    width="100"
                    class="mx-auto"
                  ></v-img>
                  <v-card-title class="justify-center text--text">
                    اختبارات دورية
                  </v-card-title>
                </v-card>
              </v-col>
              <v-col cols="12" md="3">
                <v-card elevation="0" color="transparent" class="my-5">
                  <v-img
                    src="Features/money.png"
                    width="100"
                    class="mx-auto"
                  ></v-img>
                  <v-card-title class="justify-center text--text">
                    تكلفة رمزية
                  </v-card-title>
                </v-card>
              </v-col>
            </v-row>
          </v-container>
        </v-sheet>
      </section>
      <section>
        <v-sheet
          class="pb-10"
          color="light"
        >
          <v-container>
            <h2 class="text-center primary--text my-5">كيف تبدأ التعلم عبر كتاتيب؟</h2>
            <div style="position: relative">
              <v-sparkline
                :fill="false"
                :gradient="['#58b19f ', '#58b19f ', '#f3ff41',  '#ff1f1f']"
                :line-width=".5"
                :padding="8"
                :smooth="20"
                :value="[8, 3, 5, 2, 3, 0]"
                auto-draw
              ></v-sparkline>
              <v-item-group
                class="d-flex align-center justify-space-around"
                style="position: absolute; width: 100%; top: 5.5rem;"
              >
                <v-item>
                  <v-card
                    color="error"
                    class="rounded-circle"
                    width="80"
                    height="80"
                    style="transform: translateY(4rem);"
                  >
                    <v-card-title
                      class="align-center justify-center"
                      style="height: 100%"
                    >
                      <v-icon class="white--text">mdi-account-plus</v-icon>
                    </v-card-title>
                    <v-card-text class="text-circle">
                      <router-link to="/signup">انشئ</router-link>
                      حسابك مجاناً
                    </v-card-text>
                  </v-card>
                </v-item>
                <v-item>
                  <v-card
                    color="secondary"
                    class="rounded-circle"
                    width="80"
                    height="80"
                    style="transform: translateY(1.5rem);"
                  >
                    <v-card-title
                      class="align-center justify-center"
                      style="height: 100%"
                    >
                      <v-icon class="white--text" large>mdi-playlist-check</v-icon>
                    </v-card-title>
                    <v-card-text class="text-circle middle">اشترك بالمواد التعليمية التي تحتاجها</v-card-text>
                  </v-card>
                </v-item>
                <v-item>
                  <v-card
                    color="primary"
                    class="rounded-circle"
                    width="80"
                    height="80"
                  >
                    <v-card-title
                      class="align-center justify-center"
                      style="height: 100%"
                    >
                      <v-icon
                        class="white--text"
                        large
                        style="transform: scaleX(-1)"
                      >mdi-rocket-launch</v-icon>
                    </v-card-title>
                    <v-card-text class="text-circle middle">انطلق وتابع تقدمك</v-card-text>
                  </v-card>
                </v-item>
              </v-item-group>
            </div>
          </v-container>
        </v-sheet>
      </section>
      <section>
        <v-container>
          <h2 class="text-center primary--text mt-5 mb-10">التصنيفات</h2>
          <p
            v-if="categories.length == 0"
            class="text-cente"
          >لا يوجد تصنيفات</p>
          <v-row v-else>
            <v-col
              v-for="(cat, k) in categories"
              :key="k"
              cols="12"
              md="4"
            >
              <v-card
                class="mx-auto"
                max-width="344"
                :to="`/category/${cat.id}`"
              >
                <v-card-title class="justify-center">
                  {{ cat.name }}
                </v-card-title>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
      </section>
      <section class="py-10">
        <v-container>
          <h2 class="text-center primary--text mt-5 mb-10">من المدونة</h2>
          <v-row>
            <v-col cols="12" md="4">
              <v-card
                class="mx-auto"
                max-width="344"
              >
                <v-img
                  src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg"
                  height="200px"
                ></v-img>

                <v-card-title>
                  تعلم كيف تنظم وقتك
                </v-card-title>

                <v-card-subtitle>
                  <span class="me-3">
                    <v-icon small>mdi-tag</v-icon>
                    إدارة
                  </span>
                  <span>
                    <v-icon small>mdi-calendar</v-icon>
                    10-11-2021
                  </span>
                </v-card-subtitle>
                <v-card-text>
                  هذا النص يمكن أن يتم تركيبه على أي تصميم دون مشكلة فلن يبدو وكأنه نص منسوخ، غير منظم، غير منسق، أو حتى غير مفهوم. لأنه مازال نصاً بديلاً و...
                </v-card-text>
              </v-card>
            </v-col>
            <v-col cols="12" md="4">
              <v-card
                class="mx-auto"
                max-width="344"
              >
                <v-img
                  src="https://cdn.vuetifyjs.com/images/cards/cooking.png"
                  height="200px"
                ></v-img>

                <v-card-title>
                  ابتعد عن هذه الأطعمة الآن
                </v-card-title>

                <v-card-subtitle>
                  <span class="me-3">
                    <v-icon small>mdi-tag</v-icon>
                    صحة
                  </span>
                  <span>
                    <v-icon small>mdi-calendar</v-icon>
                    10-11-2021
                  </span>
                </v-card-subtitle>
                <v-card-text>
                  هذا النص يمكن أن يتم تركيبه على أي تصميم دون مشكلة فلن يبدو وكأنه نص منسوخ، غير منظم، غير منسق، أو حتى غير مفهوم. لأنه مازال نصاً بديلاً و...
                </v-card-text>
              </v-card>
            </v-col>
            <v-col cols="12" md="4">
              <v-card
                class="mx-auto"
                max-width="344"
              >
                <v-img
                  src="https://cdn.vuetifyjs.com/images/cards/road.jpg"
                  height="200px"
                ></v-img>

                <v-card-title>
                  طريق النجاح يحتاج الى خطة
                </v-card-title>

                <v-card-subtitle>
                  <span class="me-3">
                    <v-icon small>mdi-tag</v-icon>
                    إدارة
                  </span>
                  <span>
                    <v-icon small>mdi-calendar</v-icon>
                    10-11-2021
                  </span>
                </v-card-subtitle>
                <v-card-text>
                  هذا النص يمكن أن يتم تركيبه على أي تصميم دون مشكلة فلن يبدو وكأنه نص منسوخ، غير منظم، غير منسق، أو حتى غير مفهوم. لأنه مازال نصاً بديلاً و...
                </v-card-text>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
      </section>
    </v-main>
  </div>
</template>

<script>
export default {
  layout: 'stretch',
  data () {
    return {
      colors: [
        '#3B3B98',
        '#B33771',
        '#F97F51',
        '#82589F',
        '#58B19F',
      ],
      slides: [
        'تعلم',
        'بسهولة',
        'بأيِّ وقت',
        'بأيِّ مكان',
        'كتاتيب',
      ],
      categories: []
    }
  },
  head() {
    return {
      title: "الرئيسية"
    };
  },
  computed: {
    isAuthenticated () {
      return this.$store.getters.isAuthenticated;  // it check if user isAuthenticated
    }
  },
  mounted () {
    this.getCategories()
  },
  methods: {
    getCategories () {
      this.$axios.$get('/api/Category/TopLevelCategories')
        .then((res) => {
          if (res.result.length > 0) {
            this.categories = res.result
          }
        })
        .catch((err) => {
          console.log(err)
        })
    },
  }
}
</script>

<style scoped lang="scss">
  .header-text {font-size: 1.5em;}
  .v-list-item__title {line-height: 1.8;}
  .effect {
    position: relative;
    overflow: hidden;
    animation: effect 5s linear infinite alternate;
  }
  @keyframes effect {
    1%,5% {transform: scale(1.1);}
    6% {transform: scale(1);}
  }
  .scale-animate {animation: scalerotate 5s linear infinite alternate}
  @keyframes scalerotate {
    0% {transform: translateY(10px);}
    50% {transform: translateY(0px);}
    100% {transform: translateY(-10px);}
  }
  .text-circle {
    position: absolute;
    top: -2.5rem;
    right: -2.3rem;
    width: 200px;
    font-size: 16px;
    &.middle {
      top: 6rem;
      right: -4rem;
      text-align: center;
    }
  }
</style>
