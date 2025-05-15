<script>
export default {
  name: "DetailsComp",
  computed: {
    isLoggedIn() {
      return !!localStorage.getItem('token'); //So, !! ensures that isLoggedIn is always a boolean (true or false), which is useful for v-if in your template.
    }
  },
  mounted() {
    if (!this.isLoggedIn) {
      this.$emit('setActiveComp', 'SignIn');
    }
  }
};
</script>

<template>
  <div>
    <div v-if="isLoggedIn">
      This is the detailsPage. You are logged in!
    </div>
    <div v-else>
      You are not logged in.
    </div>
  </div>
</template>