<template>
  <v-layout column justify-center>
    <v-flex xs12 sm8 md6 lg12>
      <v-card>
        <v-card-title class="headline">Detail users</v-card-title>
        <v-card-text>
      
          <hr class="my-1">
            
            <div class="user">
              <h3>{{ name }}</h3>
              <h3>Email : {{ website }}</h3>
              <p>Address: </p>
              <p>@{{ address }}</p>
              <p><nuxt-link to="/users">Back tou users</nuxt-link></p>
            </div>

        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>


    

<script>
import axios from 'axios'

export default {
  data: () => ({
    breadcrumbs: [
      {
        text: 'Dashboard',
        disabled: false
      },
      {
        text: 'Users',
        disabled: false
      },
      {
        text: 'Detail',
        disabled: true
      }
    ]
  }),

  validate ({ params }) {
    return !isNaN(+params.id)
  },

  async asyncData ({ params, error }) {
    try {
      const { data } = await axios.get(`https://jsonplaceholder.typicode.com/users/${+params.id}`)
      // const { data } = await axios.get(`http://localhost:8000/api/v1/companies/${+params.id}`)
      return data
    } catch (e) {
      error({ message: 'User not found', statusCode: 404 })
    }
  }
}
</script>


