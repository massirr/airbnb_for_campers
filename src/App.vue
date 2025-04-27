<script>
    import NavBar from '@/components/NavbarComp.vue';

    import Account from '@/pages/accountPage.vue';
    import Bookings from '@/pages/bookingsPage.vue';
    import Camps from '@/pages/campsPage.vue';
    import Home from '@/pages/homePage.vue';
    
    export default {
      name: 'App',
      data() {
        return {
          activePage: "Camps",
          bookedItems: [], // to pass to the bookings page
          bookedState: []
        }
      },
      components: {
        Account,
        Bookings,
        Camps,
        Home,
        NavBar
      },
      methods: {
        setActivePage(page) { // the word page comes from NavBarComp
          this.activePage = page;
        },
        handleBooked(item) {
          if (!this.bookedItems.includes(item)) {
            this.bookedItems.push(item)
          }// adding the booked camp in the array --> don't forget to change if an object.
        },
        handleState(item) {
          if (!this.bookedState.includes(item)) { // if object, use different method
            this.bookedState.push(item)
          }
        },
      }
    }
</script>

<!--for tailwind@3 to work, you need tailwind.css in assets, postcss.config.js and tailwind.config.js -> make sure you know the version you are working with-->
<template>
  <div id="app">
    <NavBar @setActivePage="setActivePage" :activePage="activePage"/> <!--send the active page data as a prop-->

    <!--pages-->
    <Account v-if="activePage == 'Account'"/>
    <Bookings v-if="activePage == 'Bookings'" @bookedState="handleState" :bookedItems="bookedItems"/>
    <Camps v-if="activePage == 'Camps'" @booked="handleBooked" :bookedState="bookedState"/> <!--the booked item is handled here and taken to booking as a prop the state is binded to the prop in Camps-->
    <Home v-if="activePage == 'Home'" @setActivePage="setActivePage"/> <!--View all camps is only active on the home page-->
  </div>
</template>

