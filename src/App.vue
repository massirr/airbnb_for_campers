<script>
    import NavBar from '@/components/NavBarComp.vue';

    import Account from '@/pages/accountPage.vue';
    import Camps from '@/pages/campsPage.vue';
    import Home from '@/pages/homePage.vue';
    
    export default {
      name: 'App',
      data() {
        return {
          activePage: "Home"
        }
      },
      components: {
        Account,
        Camps,
        Home,
        NavBar
      },
      methods: {
        setActivePage(page) { // the word page comes from NavBarComp
          this.activePage = page;
        }
      }
    }
</script>

<!--for tailwind@3 to work, you need tailwind.css in assets, postcss.config.js and tailwind.config.js -> make sure you know the version you are working with-->
<template>
  <div id="app">
    <NavBar @setActivePage="setActivePage" :activePage="activePage"/> <!--send the active page data as a prop-->

    <!--pages-->
    <Account v-if="activePage == 'Account'"/>
    <Camps v-if="activePage == 'Camps'"/>
    <Home v-if="activePage == 'Home'" @setActivePage="setActivePage"/>
  </div>
</template>

