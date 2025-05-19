<script>
    import SignIn from '@/components/SigninComp.vue';
    import Details from '@/components/DetailsComp.vue';

    export default {
      name: 'accountPage',
      computed: {
        //So, !! ensures that isLoggedIn is always a boolean (true or false), which is useful for v-if in the template.
        isLoggedIn() {
          const token = localStorage.getItem('token');
          if (!token) return false;
          try {
            const payload = JSON.parse(atob(token.split('.')[1]));
            // exp is in seconds, Date.now() is in ms
            const valid =  payload.exp * 1000 > Date.now();
            if (!valid) {
              localStorage.removeItem('token');
            }
            return valid; // the valid token if it hasn't expired
          } catch (e) {
            localStorage.removeItem('token');
            return false;
          }
        }
      },
      mounted() {
        if (this.isLoggedIn) {
          this.activePage = this.isLoggedIn ? "Details" : "SignIn"; // if else statement
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
      <Details v-if="activePage == 'Details'" @setActiveComp="setActiveComp" />
    </div>
</template>