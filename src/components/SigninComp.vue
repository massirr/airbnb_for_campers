<script>
export default {
  name: "SigninComp",
  data() {
    return {
      identifier: '', // can be email or username
      password: '',
      error: '',
    };
  },
  methods: {
    async handleLogin() {
      try {
        this.error = ""; // Clear previous error at start
        const res = await fetch('http://localhost:3000/api/auth/signin', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ identifier: this.identifier, password: this.password }),
        });
  
        const data = await res.json();
  
        if (!res.ok) throw new Error(data.message || 'Login failed');
  
        localStorage.setItem('token', data.token);
        this.$emit('setActiveComp', 'Details');
      } catch (err) {
        this.error = err.message;
        this.identifier = ''; //clears the inputs when there is an error
        this.password = '';
      }
    },
  },
};
</script>

<template>
  <div class="min-h-screen bg-blue-100 flex items-center justify-center p-8">
    <div class="max-w-lg w-full bg-gray-200 rounded-2xl shadow-2xl p-12">
      <h2 class="text-4xl font-bold text-gray-900 mb-8 text-center">Sign In</h2>

      <!-- @submit: Listens for the form's submit event -->
      <!-- v-model="email" binds the input's value to the email data property in real time-->
      <form @submit.prevent="handleLogin" class="space-y-6">
        <div>
          <label class="block text-lg font-medium text-gray-700 mb-2">Email or Username</label>
          <input 
            type="text" v-model="identifier" placeholder="Enter your email or username"
            class="w-full px-6 py-4 border border-gray-300 rounded-lg"
          />
        </div>
        <div>
          <label class="block text-lg font-medium text-gray-700 mb-2">Password</label>
          <input 
            type="password" v-model="password" placeholder="*******"
            class="w-full px-6 py-4 border border-gray-300 rounded-lg"
          />
        </div>
        <p v-if="error" class="text-red-500">{{ error }}</p>
        <button class="w-full bg-green-500 hover:bg-green-600 text-white font-bold py-4 rounded-lg text-xl">
          Sign In
        </button>
      </form>
    </div>
  </div>
</template>