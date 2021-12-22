<template>
  <v-main class="pa-0">
    <v-data-table
      :headers="headers"
      :items="data"
      :search="search"
      class="elevation-1"
      no-data-text="لا يوجد صلاحيات حتى الآن.."
      no-results-text="لم يتم العثور لى نتائج مطابقة.."
      loading-text="انتظر لحظة.."
      items-per-page-all-text="عدد الصفحات"
      items-per-page-text="كم سطر في الصفحة:"
      page-text="نص"
    >
      <template v-slot:top>
        <v-toolbar
          flat
        >
          <v-toolbar-title>الصلاحيات</v-toolbar-title>
          <v-divider
            class="mx-4"
            inset
            vertical
          ></v-divider>
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="ابحث عن صلاحيات"
            single-line
            hide-details
            class="me-4"
          ></v-text-field>
          <v-dialog
            v-model="dialog"
            max-width="500px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                color="primary"
                dark
                class="mb-2"
                v-bind="attrs"
                v-on="on"
              >
                إضافة صلاحية
              </v-btn>
            </template>
            <v-card class="pb-3">
              <v-form
                ref="roles"
                lazy-validation
              >
                <v-card-title>
                  <span>{{ formTitle }}</span>
                </v-card-title>

                <v-card-text>
                  <v-container>
                    <v-text-field
                      v-model="roleName"
                      label="وصف الصلاحية (اسم)"
                      :rules="[rules.required]"
                    ></v-text-field>
                  </v-container>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn
                    color="blue darken-1"
                    text
                    @click="close"
                  >
                    إلغاء
                  </v-btn>
                  <v-btn
                    v-if="editedIndex == -1"
                    color="primary"
                    @click="addRole()"
                  >
                    إضافة
                  </v-btn>
                  <v-btn
                    v-else
                    color="primary"
                    @click="submitEdit()"
                  >
                    حفظ
                  </v-btn>
                </v-card-actions>
              </v-form>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title>هل انت متأكد انك تريد حذف هذا العنصر؟</v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete">إلغاء</v-btn>
                <v-btn color="error" @click="deleteItemConfirm()">حذف</v-btn>
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:[`item.actions`]="{ item }">
        <v-btn
          icon
          class="mx-2 success--text"
          @click="editItem(item)"
        >
          <v-icon small>mdi-pencil</v-icon>
        </v-btn>
        <v-btn
          icon
          class="error--text"
          @click="deleteItem(item)"
        >
          <v-icon small>mdi-delete</v-icon>
        </v-btn>
      </template>
    </v-data-table>
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
        {{ msgText }}
      </v-alert>
    </v-snackbar>
    <v-snackbar
      v-model="info"
      :timeout="3000"
      elevation="0"
      color="transparent"
    >
      <v-alert
        dismissible
        elevation="3"
        border="left"
        type="info"
        width="300"
      >
        {{ msgText }}
      </v-alert>
    </v-snackbar>
  </v-main>
</template>

<script>
export default {
  data () {
    return {
      dialog: false,
      dialogDelete: false,
      rules: {
        required: value => !!value || 'هذا الحقل مطللوب ..!',
      },
      search: '',
      headers: [
        {
          text: 'الوصف',
          align: 'start',
          value: 'name',
        },
        { text: 'عمليات', value: 'actions', sortable: false },
      ],
      roleName: '',
      data: [],
      editedIndex: -1,
      message: false,
      info: false,
      msgText: '',
    }
  },
  head () {
    return {
      title: 'الصلاحيات'
    };
  },
  computed: {
    getUser () {
      return this.$store.getters.getUserInfo.userName;  // it get current user
    },
    formTitle () {
      return this.editedIndex === -1 ? 'اضف صلاحية جديدة' : 'تعديل صلاحية'
    },
  },

  watch: {
    dialog (val) {
      val || this.close()
    },
    dialogDelete (val) {
      val || this.closeDelete()
    },
  },

  mounted () {
    this.getRoles()
  },

  methods: {

    // Get Roles Data From API
    getRoles () {
      this.$axios.$post('/api/Dashboard/AllRoles', {
        pageNumber: 0,
        pageSize: 10
      })
        .then((res) => {
          if (res.length > 0) {
            this.data = res
          }
        })
        .catch((err) => {
          console.log(err)
        })
    },

    // Add New Categories
    addRole () {
      if (this.$refs.roles.validate()) {
        this.$axios.$post('/api/Dashboard/CreateRole', { name: this.roleName })
          .then(() => {
            this.msgText = 'تمت إضافة الصلاحية بنجاح.'
            this.message = true
            this.getRoles()
            this.close()
          })
          .catch((err) => {
            console.log(err)
          })
      }
    },

    // Edit Item From Table
    editItem (item) {
      this.editedIndex = this.data.indexOf(item)
      this.roleName = item.name
      this.dialog = true
    },

    // Submit Edit
    submitEdit () {
      if (this.$refs.roles.validate()) {
        const role = {
          id: this.data[this.editedIndex].id,
          name: this.roleName
        }
        this.$axios.$post('/api/Dashboard/UpdateRole', role)
          .then(() => {
            this.msgText = 'تم تعديل المعلومات بنجاح.'
            this.message = true
            this.getRoles()
            this.close()
          })
          .catch((err) => {
            console.log(err)
          })
      }
    },

    // Delete Item From Table
    deleteItem (item) {
      this.editedIndex = this.data.indexOf(item)
      this.dialogDelete = true
      console.log(item)
    },

    // Delete Confirmation
    // deleteItemConfirm () {
    //   this.$axios.$post('/api/Dashboard/', this.data[this.editedIndex].id)
    //   .then(() => {
    //     this.msgText = 'تم حذف الصلاحية بنجاح.'
    //     this.message = true
    //     this.data.splice(this.editedIndex, 1)
    //     this.closeDelete()
    //   })
    //   .catch((err) => {
    //     console.log(err)
    //   })
    // },

    close () {
      this.dialog = false
      this.$nextTick(() => {
        this.editedIndex = -1
        this.$refs.roles.reset()
      })
    },

    closeDelete () {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedIndex = -1
      })
    },

  },
}
</script>
