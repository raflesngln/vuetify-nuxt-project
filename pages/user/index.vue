<template>
    <div>
      <!-- Snackbar -->
      <v-snackbar
        v-model="snack"
        :timeout="3000"
        :top="y === 'top'"
        :color="snackColor">
        {{ snackText }}
        <v-btn flat @click="snack = false">Close</v-btn>
      </v-snackbar>

    <h1> <v-icon color="purple darken-1">dialpad</v-icon> List Users</h1>
    <v-card>
      <v-dialog v-model="dialog" width="600px" max-width="700px">
        <v-btn color="primary" dark slot="activator" class="mb-2">New Item</v-btn>
        <v-card>
          <form onsubmit="event.preventDefault()" method="POST" @submit="saveData(editedItem.id)" name="modalForm" ref="modalForm">
          <v-card-title>
            <span class="headline">{{ formTitle }}</span>
          </v-card-title>
          <v-card-text>
            <v-container grid-list-md>
              <v-layout wrap>
                <v-flex xs12 sm6 md4>
                  <v-text-field label=" Name" v-model="editedItem.name"></v-text-field>
                </v-flex>
                <v-flex xs12 sm6 md4>
                  <v-text-field label="Address" v-model="editedItem.address"></v-text-field>
                </v-flex>
                <v-flex xs12 sm6 md4>
                  <v-text-field label="Website" v-model="editedItem.website"></v-text-field>
                </v-flex>
                <v-flex xs12 sm6 md4>
                  <v-text-field label="Email" v-model="editedItem.email"></v-text-field>
                </v-flex>
                <v-flex xs12 sm6 md4>
                  <v-dialog
                              ref="dialog"
                              v-model="modal"
                              :return-value.sync="date"
                              persistent
                              lazy
                              full-width
                              width="300px"
                            >
                              <v-text-field
                                slot="activator"
                                v-model="editedItem.created_at"
                                label="Date Created"
                                prepend-icon="event"
                                readonly
                              ></v-text-field>
                              <v-date-picker v-model="date" scrollable>
                                <v-spacer></v-spacer>
                                <v-btn flat color="primary" @click="modal = false">Cancel</v-btn>
                                <v-btn flat color="primary" @click="$refs.dialog.save(date)">OK</v-btn>
                              </v-date-picker>
                    </v-dialog>
                </v-flex>
              </v-layout>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="error" flat @click.native="close">Cancel</v-btn>
            <v-btn color="success" flat type="submit" >{{actionforChange}}</v-btn>
          </v-card-actions>
          </form>
        </v-card>
      </v-dialog>
      <!-- TABLE -->
      <v-data-table
        :headers="headers"
        :items="items"
        :search="search"
        :pagination.sync="pagination"
        :total-items="totalItems"
        :loading="loading"
        class="elevation-1"
      >
        <template slot="items" slot-scope="props">
          <td>{{ props.item.name }}</td>
          <td class="text-xs-left">{{ props.item.address }}</td>
          <td class="text-xs-left">{{ props.item.website }}</td>
          <td class="text-xs-left">{{ props.item.email }}</td>
          <td class="text-xs-right">{{ props.item.created_at }}</td>
          <td class="justify-center layout px-0">
            <v-btn icon class="mx-0" @click="editItem(props.item)">
              <v-icon color="teal">edit</v-icon>
            </v-btn>

            <v-btn icon class="mx-0" @click="confirmDelete(props.item.id)">
              <v-icon color="red darken-12">delete</v-icon>
            </v-btn>
          </td>
        </template>
        <template slot="no-data">
          <v-btn color="primary" @click="getData">Reset</v-btn>
        </template>
      </v-data-table>
      <!-- END OF TABLE -->
      </v-card>

    <!-- Modal delete -->
      <v-dialog
        v-model="dialogDel"
        max-width="290"
      >
        <v-card>
          <v-card-title class="headline">Delete Confirmation</v-card-title>
  
          <v-card-text>
           Are Sure delete data with ID  {{ indexID }}  ? 
          </v-card-text>
  
          <v-card-actions>
            <v-spacer></v-spacer>
  
            <v-btn
              color="red darken-10"
              flat="flat"
              @click="cancelDelete">
              NO
            </v-btn>
            <v-btn
              color="green darken-1"
              flat="flat"
              @click="deleteItem(indexID)">
              YES
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
      </div>
</template>

<script>
import axios from 'axios'
import VueSweetalert2 from 'vue-sweetalert2'

export default {
  components: {
    VueSweetalert2
  },
  data: () => ({
    snack: false,
    y: 'top',
    snackColor: '',
    snackText: '',
    date: null,
    menu: false,
    modal: false,
    menu2: false,

    dialog: false,
    dialogDel: false,
    search: '',
    totalItems: 0,
    items: [],
    indexID: '',
    loading: true,
    pagination: {},
    editedIndex: -1,
    headers: [
      {
        text: 'Name',
        align: 'left',
        sortable: true,
        value: 'name'
      },
      { text: 'Address', value: 'address' },
      { text: 'Website', value: 'website' },
      { text: 'Email', value: 'email', sortable: false },
      { text: 'created_at', value: 'created_at' },
      { text: 'Actions', value: 'name', sortable: false }
    ],
    editedItem: {
      name: '',
      address: '',
      website: '',
      email: '',
      created_at: ''
    },
    defaultItem: {
      name: '',
      address: '',
      website: '',
      email: '',
      created_at: ''
    }
  }),

  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'New Items' : 'Edit Item'
    },
    actionforChange () {
      return this.editedIndex === -1 ? 'Save Data' : 'Update Data'
    }
  },

  watch: {
    dialog (val) {
      val || this.close()
    }
  },
  created () {
    this.getData()
  },
  methods: {
    // --------getdata from database------------
    getData () {
      this.loading = false
      let self = this
      axios.get('http://localhost:8000/api/v1/companies')
        .then(function (response) {
          self.items = response.data
          console.log(response.data)
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    // --------Edit Data------------
    editItem (item) {
      this.editedIndex = this.items.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },
    // --------getdata from database------------
    deleteItem (item) {
      let id = item
      axios.delete('http://localhost:8000/api/v1/companies/' + id)
        .then(response => {
          this.getData()
          this.dialogDel = false
          this.snacbarinfo('error', 'Data Success Delete !')
          return response
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    // --------confirm before delete-----------
    confirmDelete (item) {
      let id = item
      this.indexID = id
      this.dialogDel = true
    },
    // --------Cancel delete and close modal------------
    cancelDelete () {
      this.dialogDel = false
    },
    // --------sweet alert for information------------
    sweetAlertView () {
      // $swal function calls SweetAlert into the application with the specified configuration.
      this.$swal({
        title: 'What is your Name?',
        input: 'text',
        inputPlaceholder: 'Enter your name here',
        showCloseButton: true
      })
    },
    // --------close modal dialog------------
    close () {
      this.dialog = false
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      }, 300)
    },
    // --------save data function------------
    saveData (item) {
      this.loading = true
      let id = item
      const dataPost = {}
      dataPost['name'] = this.editedItem.name
      dataPost['address'] = this.editedItem.address
      dataPost['website'] = this.editedItem.website
      dataPost['email'] = this.editedItem.email
      dataPost['created_at'] = '2018-03-09 11:17:33'
      dataPost['updated_at'] = '2018-03-28 01:34:11'

      if (this.editedIndex > -1) {
        // Object.assign(this.items[this.editedIndex], this.editedItem)
        // alert('Update Data in database')
        axios.put('http://localhost:8000/api/v1/companies/' + id, dataPost)
          .then(response => {
            this.snacbarinfo('success', 'Data Success Update')
            this.getData()
            // console.log(response)
            return response
          })
          .catch(function (error) {
            console.log(error)
          })
      } else {
        // this.items.push(this.editedItem)
        // alert('Adding Data to database')
        axios.post('http://localhost:8000/api/v1/companies', dataPost)
          .then(response => {
            this.snacbarinfo('info', 'Data Success Adding')
            this.getData()
            // console.log(response.data)
            return response
          })
          .catch(function (error) {
            console.log(error)
          })
      }
      this.close()
    },
    // --------Snackbar notify-----------
    snacbarinfo (color, text) {
      this.snack = true
      this.snackColor = color
      this.snackText = text
    }
  }

}
</script>

<style scopped>
.v-snack--top {
    top: 4px !important;
}
</style>
