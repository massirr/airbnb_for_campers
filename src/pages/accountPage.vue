<script>
    import SignIn from '@/components/SigninComp.vue';
    import Details from '@/components/DetailsComp.vue';

    export default {
      name: 'accountPage',
      computed: {
        isLoggedIn() {
          return !!localStorage.getItem('token'); //So, !! ensures that isLoggedIn is always a boolean (true or false), which is useful for v-if in your template.
        }
      },
      mounted() {
        if (this.isLoggedIn) {
          this.activePage = "Details";
        }
      },
      components: {
        SignIn,
        Details
      },
      data() {
        return {
          activePage: "SignIn",
        }
      },
      methods: {
        setActiveComp(page) {
          this.activePage = page;
        },
      }
    }
</script>

<template>
    <div>
      <SignIn v-if="activePage == 'SignIn'" @setActiveComp="setActiveComp"/>
      <Details v-if="activePage == 'Details'"/>
    </div>
</template>