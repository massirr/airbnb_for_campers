<script>
      export default {
      name: "NavbarComp",
      data() {
        return {
          items: [
            { label: "Username", type: "text", placeholder: "Enter your username" },
            { label: "Email", type: "email", placeholder: "Enter your email" },
            { label: "Password", type: "password", placeholder: "*******" },
          ],
        }
      }
    }
</script>

<template>
    <div class="min-h-screen bg-blue-100 flex items-center justify-center p-4">
      <div class="max-w-md w-full bg-gray-200 rounded-xl shadow-lg p-8">
        <h2 class="text-2xl font-bold text-gray-900 mb-6 text-center">Sign In</h2>
        
        <form class="space-y-4">
          <div v-for="(item, index) in items" :key="index">
            <label class="block text-sm font-medium text-gray-700">{{ item.label }}</label>
            <!--browsers automatically apply validation rules specific to email addresses with type="email", for passwords too-->
            <input 
              :type="item.type"
              :placeholder="item.placeholder"
              class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:border-green-500"
            />
          </div>

          <div class="flex items-center justify-between">
            <label class="flex items-center">
              <input type="checkbox" class="accent-gray-300"/>
              <span class="ml-2 text-sm text-gray-600">Remember me</span>
            </label>
            <button class="text-sm text-green-500 mb-2 hover:text-green-600">Forgot password?</button>
          </div>

          <button class="w-full bg-green-500 hover:bg-green-600 text-white font-medium py-2.5 rounded-lg transition-colors">
            Sign In
          </button>
        </form>

        <div class="mt-6 text-center text-sm text-gray-600">
          Don't have an account? 
          <button class="text-green-500 hover:text-green-600 font-medium">Sign up</button>
        </div>
      </div>
    </div>
</template>