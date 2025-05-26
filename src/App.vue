<script>
    import NavBar from '@/components/NavbarComp.vue';

    import Account from '@/pages/accountPage.vue';
    import Bookings from '@/pages/bookingsPage.vue';
    import Camps from '@/pages/campsPage.vue';
    import CampInfo from '@/pages/campInfoPage.vue';
    import Create from '@/pages/createCampPage.vue';
    import Home from '@/pages/homePage.vue';

    export default {
      name: 'App',
      data() {
        return {
          activePage: "Camps",
          campData: null // Store the camp data for CampInfo page
        };
      },
      components: {
        Account,
        Bookings,
        Camps,
        CampInfo,
        Create,
        Home,
        NavBar
      },
      methods: {
        setActivePage(payload) {
          if (typeof payload === 'object' && payload.page === 'CampInfo') {
            this.activePage = payload.page;
            this.campData = payload.camp; // Store the camp data
          } else {
            this.activePage = payload;
          }
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
    <CampInfo v-if="activePage == 'CampInfo'" @setActivePage="setActivePage" :camp="campData"/>
    <Create v-if="activePage == 'Create'" @setActivePage="setActivePage"/>
    <Home v-if="activePage == 'Home'" @setActivePage="setActivePage"/> <!--View all camps is only active on the home page-->
  </div>
</template>

