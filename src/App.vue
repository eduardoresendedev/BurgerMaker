<script>
import NavBar from './components/NavBar.vue';
import Footer from './components/Footer.vue'

export default {
  components: {
    NavBar,Footer,
  },
  data() {
    return {
      logo_src: '/img/logo.png',
      app_name:"Make Your Burguer",
    }
  }
}


</script>

<template>
  <NavBar :logo="logo_src" :alt="app_name" />
  <RouterView />
  <Footer />
</template>

<style>
* { 
  border: 0;
  font-family: Helvetica;
  margin: 0;
  padding: 0; 
  box-sizing: border-box;
}

.main-container {
  margin: 90px;
  min-height: 250px;
}

h1 {
  text-align: center;
  font-size: 42px;
  margin-bottom: 30px;
  color: #222;
}

</style>