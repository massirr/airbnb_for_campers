<script>
    import NavBar from '@/components/NavBarComp.vue';

    import Account from '@/pages/accountPage.vue';
    import Home from '@/pages/homePage.vue';
    
    export default {
      name: 'App',
      data() {
        return {
          activePage: "home"
        }
      },
      components: {
        Account,
        Home,
        NavBar
      },
      methods: {
        setActivePage(page) { // the word page comes from navbar
          this.activePage = page;
        }
      }
    }
</script>

<!--for tailwind@3 to work, you need tailwind.css in assets, postcss.config.js and tailwind.config.js -> make sure you know the version you are working with-->
<template>
  <div id="app">
    <NavBar @setActivePage="setActivePage"/>

    <!--pages-->
    <Home v-if="activePage == 'home'"/>
    <Account v-if="activePage == 'account'"/>
  </div>
</template>

