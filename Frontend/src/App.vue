<template>
  <div id="app">
    <HeaderComponent/>
      <!--<LastArticles/> -->
      <!--No cargo el componente central sino el router -->
      <router-view></router-view>

      <div class="clearfix"></div>

    <Footer/>
  </div>
</template>

<script>

import HeaderComponent from "./components/HeaderComponent";

import Footer from "./components/Footer";

export default {
  name: 'App',
  //Aqui se carga todos los componentes que usare, es el comp principal
  components: {
    HeaderComponent,
    Footer

  }
}
</script>

<style>
  /*
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
*/
  @import './assets/css/styles.css';

</style>
