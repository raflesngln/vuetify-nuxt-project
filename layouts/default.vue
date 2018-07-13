<template>
  <v-app dark>
    <v-navigation-drawer
      :mini-variant.sync="miniVariant"
      :clipped="clipped"
      v-model="drawer"
      fixed
      app
    >
    <!-- avatar -->
     <v-list-tile avatar>
            <v-list-tile-avatar>
              <img src="https://randomuser.me/api/portraits/men/85.jpg" >
            </v-list-tile-avatar>

            <v-list-tile-content>
              <v-list-tile-title>Administrator</v-list-tile-title>
            </v-list-tile-content>
          </v-list-tile>
        </v-list>
        <!-- avatar -->

<!-- MENU -->
      <v-list>
        <v-list-tile
          router
          :to="item.to"
          :key="i"
          v-for="(item, i) in items"
          exact
        >
          <v-list-tile-action>
            <v-icon v-html="item.icon"></v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title v-text="item.title"></v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>

        <!-- SUB MENU -->
          <v-list-group
          prepend-icon="account_circle"
          value="true"
        >
          <v-list-tile slot="activator">
            <v-list-tile-title>Master</v-list-tile-title>
          </v-list-tile>
  
          <v-list-group
            no-action
            sub-group
            value="true"
          >
            <v-list-tile slot="activator">
              <v-list-tile-title>Users</v-list-tile-title>
            </v-list-tile>
            <!-- sub menu master user -->
                  <v-list-tile
                    router
                    :to="user.to"
                    :key="i"
                    v-for="(user, i) in users"
                    exact
                  >
                    <v-list-tile-action>
                      <v-icon v-html="user.icon"></v-icon>
                    </v-list-tile-action>
                    <v-list-tile-content>
                      <v-list-tile-title v-text="user.title"></v-list-tile-title>
                    </v-list-tile-content>
                  </v-list-tile>
          </v-list-group>
  
          <v-list-group
            sub-group
            no-action
          >
            <v-list-tile slot="activator">
              <v-list-tile-title>Products</v-list-tile-title>
            </v-list-tile>

                  <v-list-tile
                    router
                    :to="product.to"
                    :key="i"
                    v-for="(product, i) in products"
                    exact
                  >
                    <v-list-tile-action>
                      <v-icon v-html="product.icon"></v-icon>
                    </v-list-tile-action>
                    <v-list-tile-content>
                      <v-list-tile-title v-text="product.title"></v-list-tile-title>
                    </v-list-tile-content>
                  </v-list-tile>

          </v-list-group>
        </v-list-group>
          <!-- END OF SUB MENU -->

      </v-list>
<!-- END OF MENU -->

<!-- Menu Toolbar -->
    </v-navigation-drawer>
    <v-toolbar fixed app :clipped-left="clipped">
      <v-toolbar-side-icon @click="drawer = !drawer"></v-toolbar-side-icon>
      <v-btn
        icon
        @click.stop="miniVariant = !miniVariant"
      >
        <v-icon v-html="miniVariant ? 'chevron_right' : 'chevron_left'"></v-icon>
      </v-btn>
      <v-btn
        icon
        @click.stop="clipped = !clipped"
      >
        <v-icon>web</v-icon>
      </v-btn>

      <v-toolbar-title v-text="title"></v-toolbar-title>
      <v-spacer></v-spacer>

<!-- Menu notification right -->

<!-- Menu right 1 -->
        <v-menu bottom left>
          <v-btn slot="activator" dark icon> 
              <v-badge overlap>
                <span slot="badge">3</span>
                  <v-icon large color="white red--after">notifications</v-icon>
              </v-badge>
          </v-btn>

            <v-card>
                  <v-list>
                    <v-list-tile avatar>
                      Notification
                    </v-list-tile>
                  </v-list>
                  <v-divider></v-divider>
          
                          <v-list>
                            <v-list-tile
                              v-for="(itemmenu, i) in menuNotif"
                              :key="i"
                              @click=""
                            >
                              <v-list-tile-title>{{ itemmenu.title }}</v-list-tile-title>
                            </v-list-tile>
                          </v-list>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn flat color="orange">Show all Notifications</v-btn>
                  </v-card-actions>
                </v-card>
        </v-menu>

<!-- Menu notif 2 -->
        <v-menu bottom left>
            <v-btn slot="activator" dark icon> 
                <v-icon large color="white darken-2">account_circle</v-icon>
            </v-btn>

        <v-card>
                  <v-list>
                    <v-list-tile avatar>
                      <v-list-tile-avatar>
                        <img src="https://cdn.vuetifyjs.com/images/john.jpg" alt="John">
                      </v-list-tile-avatar>
          
                      <v-list-tile-content>
                        <v-list-tile-title>John Leider</v-list-tile-title>
                        <v-list-tile-sub-title>Founder of Vuetify.js</v-list-tile-sub-title>
                      </v-list-tile-content>
                    </v-list-tile>
                  </v-list>
                  <v-divider></v-divider>
          
                          <v-list>
                            <v-list-tile
                              v-for="(itemmenu, i) in menuNotif"
                              :key="i"
                              @click=""
                            >
                              <v-list-tile-title>{{ itemmenu.title }}</v-list-tile-title>
                            </v-list-tile>
                          </v-list>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn flat @click="menu = false">Cancel</v-btn>
                    <v-btn color="primary" flat @click="menu = false">Save</v-btn>
                  </v-card-actions>
                </v-card>
            </v-menu>
          
    </v-toolbar>

    <!-- CONTENT -->
    <v-content>
      <v-container>
        <!-- <div class="container" style="background:black;margin:2px"> -->
              <nuxt />
          <!-- </div> -->
      </v-container>
    </v-content>
    <!-- END OF CONTENT -->

    
    <v-footer :fixed="fixed" app>
      <span>&copy; 2017</span>
    </v-footer>
  </v-app>
</template>

<script>
  export default {
    data () {
      return {
        clipped: false,
        drawer: true,
        fixed: false,
        miniVariant: false,
        right: true,
        rightDrawer: false,
        title: 'Vuetify and Nuxt',
        menuNotif: [
          { title: 'Settings' },
          { title: 'Message' },
          { title: 'My Profile' },
          { title: 'notification' }
        ],
        items: [
          { icon: 'apps', title: 'Home', to: '/' },
          { icon: 'bubble_chart', title: 'Monitor', to: '/monitor' }
        ],
        users: [
          { icon: 'people_outline', title: 'List user', to: '/user' },
          { icon: 'settings', title: 'Manage', to: '/users' },
          { icon: 'bubble_chart', title: 'Datatables', to: '/datatable' }
        ],
        products: [
          { icon: 'insert_drive_file', title: 'manage', to: '/products/product' },
          { icon: 'insert_drive_file', title: 'Report', to: '/products/report' }
        ]
      }
    }
  }
</script>

<style scoped>
.v-navigation-drawer {
  background-color: rgb(86, 83, 87);
  /* width: 280px !important; */
}
.v-content{
  float: left;
  padding: 58px 0px 0px 300px;
  /* padding: 64px 0px 30px 230px !important; */
}
.v-container{
  background: #f00988 !important;

}

</style>
