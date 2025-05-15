<script>
    import NavBar from '@/components/NavbarComp.vue';

    import Account from '@/pages/accountPage.vue';
    import Bookings from '@/pages/bookingsPage.vue';
    import Camps from '@/pages/campsPage.vue';
    import CampInfo from '@/pages/campInfoPage.vue';
    import Home from '@/pages/homePage.vue';

    export default {
      name: 'App',
      data() {
        return {
          activePage: "Camps",
        }
      },
      components: {
        Account,
        Bookings,
        Camps,
        CampInfo,
        Home,
        NavBar,
      },
      methods: {
        setActivePage(page) { // the word page comes from NavBarComp
          this.activePage = page;
        },
      }
    }
</script>

<!--for tailwind@3 to work, you need tailwind.css in assets, postcss.config.js and tailwind.config.js -> make sure you know the version you are working with-->
<template>
  <div>
    <NavBar @setActivePage="setActivePage" :activePage="activePage"/> <!--send the active page data as a prop-->

    <!--pages-->
    <Account v-if="activePage == 'Account'"/>
    <Bookings v-if="activePage == 'Bookings'"/>
    <Camps v-if="activePage == 'Camps'" @setActivePage="setActivePage"/>
    <CampInfo v-if="activePage == 'CampInfo'"/>
    <Home v-if="activePage == 'Home'" @setActivePage="setActivePage" @setCampPage="setActivePage"/> <!--View all camps is only active on the home page-->
  </div>
</template>

