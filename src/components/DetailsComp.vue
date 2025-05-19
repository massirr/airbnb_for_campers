<script>
export default {
  name: "DetailsComp",
  computed: {
    isLoggedIn() {
      return !!localStorage.getItem('token');
    },
    // Example: decode user info from token (if using JWT)
    user() {
      const token = localStorage.getItem('token');
      if (!token) return null;
      try {
        return JSON.parse(atob(token.split('.')[1]));
      } catch {
        return null;
      }
    }
  },
  methods: {
    logout() {
      localStorage.removeItem('token');
      this.$emit('setActiveComp', 'SignIn');
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
      <div class="flex justify-between items-center mb-6">
        <h2 class="text-2xl font-bold">Account Details</h2>
        <button @click="logout" class="bg-red-500 text-white px-4 py-2 rounded">Logout</button>
      </div>
      <div>
        <p><strong>Username:</strong> {{ user?.username }}</p>
        <p><strong>Email:</strong> {{ user?.email }}</p>
        <!-- Add more user info here -->
      </div>
    </div>
    <div v-else>
      You are not logged in.
    </div>
  </div>
</template>