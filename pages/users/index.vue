<template>
  <v-layout column justify-center>
    <v-flex xs12 sm8 md6 lg12>

      <v-card>
        <v-card-title class="headline">List Users</v-card-title>
        <v-card-text>
      
          <div class="text-xs-right">
           <v-btn color="success">Add New</v-btn>
          </div>
          <hr class="my-1">
           <ul class="users">
            <li v-for="user in users" :key="user.id">
              <nuxt-link :to="'/users/'+user.id">{{ user.name }}</nuxt-link>
            </li>
          </ul>
        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
import axios from 'axios'
export default {
  async asyncData () {
    // const { data } = await axios.get('https://jsonplaceholder.typicode.com/users')
    const { data } = await axios.get('http://localhost:8000/api/v1/companies')
    return { users: data }
  }
}
</script>

<style scoped>
.v-container{
  padding-left: 20px;
}
</style>


  //   "id": 1,
  //   "name": "Leanne Graham",
  //   "username": "Bret",
  //   "email": "Sincere@april.biz",
  //   "address": {
  //     "street": "Kulas Light",
  //     "suite": "Apt. 556",
  //     "city": "Gwenborough",
  //     "zipcode": "92998-3874",
  //     "geo": {
  //       "lat": "-37.3159",
  //       "lng": "81.1496"
  //     }
  //   },
  //   "phone": "1-770-736-8031 x56442",
  //   "website": "hildegard.org",
  //   "company": {
  //     "name": "Romaguera-Crona",
  //     "catchPhrase": "Multi-layered client-server neural-net",
  //     "bs": "harness real-time e-markets"
  //   }
  // },