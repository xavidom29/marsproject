<template>
  <!-- <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </div> -->
    <v-app style="height: 100vh; overflow:hidden;">
   
    <v-main :style="{'background-image': 'url(' + require('./assets/mars2.jpg') + ')'}">
      <Navbar/>
      <Home/>
      <Footer/>
    </v-main>

  </v-app>
</template>

<script>
import Footer from './components/Footer.vue';
import Home from './components/Home';
import Navbar from './components/Navbar';

export default {
  name: 'App',

  components: {
    Home,
    Footer,
    Navbar,
  },

  data: () => ({
    drawer: false,
  }),
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
