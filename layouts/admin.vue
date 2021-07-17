<template>
  <v-app dark>
    <div>
      <v-app-bar color="#FFFFFF" flat>
        <v-container>
          <v-img
            class="mx-2"
            src="/logo.png"
            max-height="130"
            max-width="150"
            contain
          ></v-img>
        </v-container>
        <v-spacer></v-spacer>
        <nuxt-link to="/">
        <v-btn rounded color="#6C63FF" dark outlined class="text-capitalize">Home</v-btn>
        </nuxt-link>
      </v-app-bar>
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
body {
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  background: #ecf4ff;
}
a{
  text-decoration: none;
}
</style>
