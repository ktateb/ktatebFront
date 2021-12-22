<template>
  <v-main class="pa-0">
    <v-data-table
      :headers="headers"
      :items="data"
      :search="search"
      class="elevation-1"
      no-data-text="لا يوجد مستخدمين حتى الآن.."
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
          <v-toolbar-title>المستخدمين</v-toolbar-title>
          <v-divider
            class="mx-4"
            inset
            vertical
          ></v-divider>
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="ابحث عن مستخدم"
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
                إضافة مستخدم
              </v-btn>
            </template>
            <v-card class="pb-3">
              <v-form
                ref="cats"
                lazy-validation
              >
                <v-card-title>
                  <span>{{ formTitle }}</span>
                </v-card-title>

                <v-card-text>
                  <v-container>
                    Soon
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
                    @click="addUser()"
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
              <v-card-title>هل انت متأكد انك تريد حذف هذا المستخدم؟</v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete">إلغاء</v-btn>
                <v-btn color="error" @click="deleteItemConfirm()">حذف</v-btn>
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog
            v-model="roleDialog"
            max-width="500px"
          >
            <v-card class="pb-3">
              <v-form
                ref="managerole"
                lazy-validation
                @submit.prevent=""
              >
                <v-card-title>
                  <span> إدارة صلاحيات المستخدم </span>
                </v-card-title>

                <v-card-text>
                  <v-container>
                    <v-radio-group
                      v-model="roleOption"
                      row
                      :rules="[rules.required]"
                      @change="getRoles()"
                    >
                      <v-radio
                        label="إضافة صلاحية"
                        value="add"
                      ></v-radio>
                      <v-radio
                        label="حذف صلاحية"
                        value="del"
                      ></v-radio>
                    </v-radio-group>
                    <v-autocomplete
                      v-model="role.roleName"
                      :items="roles"
                      item-text="name"
                      item-value="name"
                      label="اختر الصلاحية"
                      prepend-inner-icon="mdi-star"
                      :rules="[rules.required]"
                      no-data-text="لا يوجد كلمة مطابقة!"
                      class="ma-0"
                    ></v-autocomplete>
                  </v-container>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn
                    color="blue darken-1"
                    text
                    @click="closeRole()"
                  >
                    إلغاء
                  </v-btn>
                  <v-btn
                    color="primary"
                    @click="saveRole()"
                  >
                    حفظ
                  </v-btn>
                </v-card-actions>
              </v-form>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:[`item.actions`]="{ item }">
        <v-btn
          icon
          class="basePrimary--text"
          @click="manageRoles(item)"
        >
          <v-icon small>mdi-account-star</v-icon>
        </v-btn>
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
          text: 'اسم المستخدم',
          align: 'start',
          value: 'userName',
        },
        { text: 'الاسم الكامل', value: 'displayName' },
        { text: 'البريد', value: 'email' },
        { text: 'الهاتف', value: 'phoneNumber' },
        { text: 'الصلاحيات', value: 'roles' },
        { text: 'عمليات', value: 'actions', sortable: false },
      ],
      parent: null,
      data: [],
      roleDialog: false,
      roleOption: '',
      roles: [],
      role: {
        userName: '',
        roleName: ''
      },
      editedIndex: -1,
      message: false,
      info: false,
      msgText: '',
    }
  },
  head () {
    return {
      title: 'المستخدمين'
    };
  },
  computed: {
    getUser () {
      return this.$store.getters.getUserInfo.userName;  // it get current user
    },
    formTitle () {
      return this.editedIndex === -1 ? 'اضف مستخدم جديد' : 'تعديل بيانات مستخدم'
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
    this.getUsers()
  },

  methods: {

    // Get Categories Data From API
    getUsers () {
      this.$axios.$get('/api/Dashboard/Users')
        .then((res) => {
          if (res.length > 0) {
            res.filter(user => !user.roles.includes('Admin') ? this.data.push(user) : '')
          }
        })
        .catch((err) => {
          console.log(err)
        })
    },

    // Add New User
    // addUser () {
    //   if (this.$refs.cats.validate()) {
    //     const user = {
    //     }

    //     this.$axios.$post('/api/Dashboard/Adduser', user)
    //       .then(() => {
    //         this.msgText = 'تمت إضافة المستخدم بنجاح.'
    //         this.message = true
    //         this.getUsers()
    //         this.close()
    //       })
    //       .catch((err) => {
    //         console.log(err)
    //       })
    //   }
    // },

    // Manage Roles
    manageRoles (item) {
      this.editedIndex = this.data.indexOf(item)
      this.role.userName = item.userName
      this.roleDialog = true
    },

    // Get Roles
    getRoles () {
      this.roles = []
      this.$axios.$post('/api/Dashboard/AllRoles', {
        pageNumber: 0,
        pageSize: 10
      })
        .then((res) => {
          if (res.length > 0) {
            if (this.roleOption === 'del') {
              res.filter(role => role.name.includes(this.data[this.editedIndex].roles) ? this.roles.push(role) : '')
            } else if (this.roleOption === 'add') {
              res.filter(role => !role.name.includes(this.data[this.editedIndex].roles) ? this.roles.push(role) : '')
            }
          }
        })
        .catch((err) => {
          console.log(err)
        })
    },

    // Save User Roles After Manage It
    saveRole () {
      if (this.$refs.managerole.validate()) {
        // Add Role To User
        if (this.roleOption === 'add') {
          this.$axios.$post('​/api​/Dashboard​/AddRoleToUser', this.role)
            .then(() => {
                this.msgText = 'تمت إعطاء الصلاحية بنجاح.'
                this.message = true
                this.getUsers()
                this.roleDialog = false
            })
            .catch((err) => {
              console.log(err)
            })
        // Delete Role From User
        } else if (this.roleOption === 'del') {
          this.$axios.$post('​/api/Dashboard/DeleteRoleFromUser', this.role)
          .then(() => {
              this.msgText = 'تم حذف الصلاحية بنجاح.'
              this.message = true
              this.getUsers()
              this.roleDialog = false
          })
          .catch((err) => {
            console.log(err)
          })
        }
      }
    },

    closeRole () {
      this.roleDialog = false
      this.$nextTick(() => {
        this.editedIndex = -1
        this.$refs.managerole.reset()
      })
    },

    // Edit Item From Table
    editItem (item) {
      this.editedIndex = this.data.indexOf(item)
      this.catName = item.name
      this.parent = item.parentId
      this.dialog = true
    },

    // // Submit Edit
    // submitEdit () {
    //   if (this.$refs.cats.validate()) {
    //     const user = {

    //     }
    //     this.$axios.$post('/api/Dashboard/UpdateUser', user)
    //       .then(() => {
    //         this.msgText = 'تم تعديل المعلومات بنجاح.'
    //         this.message = true
    //         this.getUsers()
    //         this.close()
    //       })
    //       .catch((err) => {
    //         console.log(err)
    //       })
    //   }
    // },

    // Delete Item From Table
    deleteItem (item) {
      this.editedIndex = this.data.indexOf(item)
      this.dialogDelete = true
      console.log(item)
    },

    // Delete Confirmation
    deleteItemConfirm () {
      const id = new FormData
      id.append('id', this.data[this.editedIndex].id)
      this.$axios.$delete('/api/Dashboard/DeleteUser', id)
      .then(() => {
        this.msgText = 'تم حذف المستخدم بنجاح.'
        this.message = true
        this.data.splice(this.editedIndex, 1)
        this.closeDelete()
      })
      .catch((err) => {
        console.log(err)
      })
    },

    close () {
      this.dialog = false
      this.$nextTick(() => {
        this.editedIndex = -1
        this.$refs.cats.reset()
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
