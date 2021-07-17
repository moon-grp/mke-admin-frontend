<template>
  <v-app dark>
    <div>
      <v-app-bar color="#FFFFFF" raised>
        <nuxt-link to="/autos">
          <div class="text-capitalize fnt">dashboard..autos</div>
        </nuxt-link>
        <v-spacer></v-spacer>
        <nuxt-link to="autos/createproduct">
          <v-btn
            rounded
            color="#6C63FF"
            dark
            text
            class="text-capitalize mx-2 hidden-sm-and-down"
            >add car</v-btn
          >
        </nuxt-link>

        <nuxt-link to="/orders">
          <v-btn
            rounded
            color="#6C63FF"
            dark
            text
            class="text-capitalize mx-2 hidden-sm-and-down"
            >orders</v-btn
          >
        </nuxt-link>

        <nuxt-link to="/">
          <v-btn
            rounded
            color="#6C63FF"
            dark
            text
            class="text-capitalize mx-2 hidden-sm-and-down"
            >analytics</v-btn
          >
        </nuxt-link>

        <v-app-bar-nav-icon
          @click.stop="drawer = !drawer"
          class="hidden-md-and-up"
        ></v-app-bar-nav-icon>
      </v-app-bar>

      <v-navigation-drawer v-model="drawer" absolute temporary>
        <v-list nav dense>
          <v-list-item-group
            v-model="group"
            active-class="deep-purple--text text--accent-4"
          >
            <v-list-item>
              <v-list-item-title>
                <nuxt-link to="/createproduct">
                  <div class="text-capitalize navT">create product</div>
                </nuxt-link>
                  <nuxt-link to="/orders">
                  <div class="text-capitalize navT">orders</div>
                </nuxt-link>
                  <nuxt-link to="#">
                  <div class="text-capitalize navT">analytics</div>
                </nuxt-link>
              </v-list-item-title>
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-navigation-drawer>
      <v-main>
        <v-container>
          <nuxt />
        </v-container>
      </v-main>
    </div>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
       group: null,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/',
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire',
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js',
    }
  },
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@600&display=swap');
body {
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  background: #ecf4ff;
}
a {
  text-decoration: none;
}

.fnt {
  font-family: 'Poppins', sans-serif;
  font-size: 20px;
  color: #6c63ff;
}
.navT {
  color: #13274a;
}
</style>
