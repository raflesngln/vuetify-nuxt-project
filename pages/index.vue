<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 sm8 md6>
      <v-card>
        <v-card-title class="headline">Vuetify and Nuxt.js</v-card-title>
        <v-card-text>
          <p>Vuetify is a progressive Material Design component framework for Vue.js. It was designed to empower developers to create amazing applications.</p>
           <div class="text-xs-right">
            <em><small>&mdash; John Leider</small></em>
          </div>
          <hr class="my-3">

        </v-card-text>
       

        <div>
                <v-btn color="success">Success</v-btn>
                <v-btn color="error">Error</v-btn>
                <v-btn color="warning">Warning</v-btn>
                <v-btn color="info">Info</v-btn>
              </div>
              <div>
                    <v-alert
                      :value="true"
                      type="success"
                    >
                      This is a success alert.
                    </v-alert>

                    <v-alert
                      :value="true"
                      type="info"
                    >
                      This is a info alert.
                    </v-alert>

                    <v-alert
                      :value="true"
                      type="warning"
                    >
                      This is a warning alert.
                    </v-alert>

                    <v-alert
                      :value="true"
                      type="error"
                    >
                      This is a error alert.
                    </v-alert>
                  </div>
                  
      </v-card>
    </v-flex>
  </v-layout>
</template>
