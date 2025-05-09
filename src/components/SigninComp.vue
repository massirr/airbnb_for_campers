<script>
export default {
  name: "SigninComp",
  data() {
    return {
      items: [
        { label: "Username", type: "text", placeholder: "Enter your username" },
        { label: "Email", type: "email", placeholder: "Enter your email" },
        { label: "Password", type: "password", placeholder: "*******" },
      ],
    };
  },
};
</script>

<template>
  <div class="min-h-screen bg-blue-100 flex items-center justify-center p-8">
    <div class="max-w-lg w-full bg-gray-200 rounded-2xl shadow-2xl p-12">
      <h2 class="text-4xl font-bold text-gray-900 mb-8 text-center">Sign In</h2>
      
      <form class="space-y-6">
        <div v-for="(item, index) in items" :key="index">
          <label class="block text-lg font-medium text-gray-700 mb-2">{{ item.label }}</label>
          <input 
            :type="item.type"
            :placeholder="item.placeholder"
            class="w-full px-6 py-4 border border-gray-300 rounded-lg text-lg focus:outline-none focus:ring-2 focus:ring-green-500 focus:border-green-500"
          />
        </div>

        <div class="flex items-center justify-between mt-4">
          <label class="flex items-center">
            <input type="checkbox" class="w-5 h-5 text-green-500 border-gray-300 rounded focus:ring-green-500"/>
            <span class="ml-3 text-lg text-gray-600">Remember me</span>
          </label>
          <button class="text-lg text-green-500 hover:text-green-600 font-medium">
            Forgot password?
          </button>
        </div>

        <button class="w-full bg-green-500 hover:bg-green-600 text-white font-bold py-4 rounded-lg text-xl transition-all">
          Sign In
        </button>
      </form>

      <div class="mt-8 text-center text-lg text-gray-600">
        Don't have an account? 
        <button class="text-green-500 hover:text-green-600 font-bold">Sign up</button>
      </div>
    </div>
  </div>
</template>