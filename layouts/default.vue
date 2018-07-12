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
  
            <v-list-tile
              v-for="(admin, i) in users"
              :key="i"
              @click=""
            >
              <v-list-tile-title v-text="admin[0]"></v-list-tile-title>
              <v-list-tile-action>
                <v-icon v-text="admin[1]"></v-icon>
              </v-list-tile-action>
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
              v-for="(crud, i) in products"
              :key="i"
              @click=""
            >
              <v-list-tile-title v-text="crud[0]"></v-list-tile-title>
              <v-list-tile-action>
                <v-icon v-text="crud[1]"></v-icon>
              </v-list-tile-action>
            </v-list-tile>
          </v-list-group>
        </v-list-group>
          <!-- END OF SUB MENU -->

      </v-list>
<!-- END OF MENU -->

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
      <v-btn
        icon
        @click.stop="fixed = !fixed"
      >
        <v-icon>remove</v-icon>
      </v-btn>
      <v-toolbar-title v-text="title"></v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        icon
        @click.stop="rightDrawer = !rightDrawer"
      >
        <v-icon>menu</v-icon>
      </v-btn>
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

    <v-navigation-drawer
      temporary
      :right="right"
      v-model="rightDrawer"
      fixed
    >
      <v-list>
        <v-list-tile @click.native="right = !right">
          <v-list-tile-action>
            <v-icon light>compare_arrows</v-icon>
          </v-list-tile-action>
          <v-list-tile-title>Switch drawer (click me)</v-list-tile-title>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
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
        items: [
          { icon: 'apps', title: 'Home', to: '/' },
          { icon: 'bubble_chart', title: 'Users', to: '/users' },
          { icon: 'bubble_chart', title: 'Datatables', to: '/datatable' },
          { icon: 'bubble_chart', title: 'Product', to: '/product' }
        ],
        users: [
          ['List user', 'people_outline'],
          ['Manage', 'settings']
        ],
        products: [
          ['List', 'add'],
          ['manage', 'insert_drive_file'],
          ['Report', 'insert_drive_file']
        ],
        miniVariant: false,
        right: true,
        rightDrawer: false,
        title: 'Vuetify and Nuxt'
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
  background: red !important;

}

</style>
