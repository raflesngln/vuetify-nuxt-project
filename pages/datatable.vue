<template>
    <div>
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
            <v-btn icon class="mx-0" @click="deleteItem(props.item)">
              <v-icon color="pink">delete</v-icon>
            </v-btn>
          </td>
        </template>
        <template slot="no-data">
          <v-btn color="primary" @click="getData">Reset</v-btn>
        </template>
      </v-data-table>
      <!-- END OF TABLE -->
      </v-card>

    </div>
</template>

<script>
import axios from 'axios'

export default {
  data: () => ({
    date: null,
    menu: false,
    modal: false,
    menu2: false,

    dialog: false,
    search: '',
    totalItems: 0,
    items: [],
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
      address: 0,
      website: 0,
      email: 0,
      created_at: 0
    },
    defaultItem: {
      name: '',
      address: 0,
      website: 0,
      email: 0,
      created_at: 0
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
    editItem (item) {
      this.editedIndex = this.items.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem (item) {
      const index = this.items.indexOf(item)
      confirm('Are you sure you want to delete this item?') && this.items.splice(index, 1)
    },

    close () {
      this.dialog = false
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      }, 300)
    },

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
            this.getData()
            console.log(response)
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
            this.getData()
            console.log(response.data)
            return response
          })
          .catch(function (error) {
            console.log(error)
          })
      }
      this.close()
    }
  }

}
</script>