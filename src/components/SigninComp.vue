<script>
export default {
  name: "SigninComp",
  data() {
    return {
      // Common data
      registerActive: false,
      emptyFields: false,
      error: '',
      
      // Login data
      identifier: '', // email or username for login
      password: '',
      
      // Registration data
      username: '',
      email: '',
      passwordReg: '',
      confirmReg: '',
      
      // Password visibility
      showPassword: false,
      showPasswordReg: false,
      showConfirmReg: false,
    };
  },
  methods: {
    async handleLogin() {
      try {
        // Form validation
        if (!this.identifier || !this.password) {
          this.emptyFields = true;
          this.error = "Please fill in all fields";
          return;
        }
        
        this.emptyFields = false;
        this.error = ""; // Clear previous error
        
        const res = await fetch('http://localhost:3000/api/auth/signin', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ 
            identifier: this.identifier, 
            password: this.password 
          }),
        });
  
        const data = await res.json();
        if (!res.ok) throw new Error(data.message || 'Login failed');
  
        localStorage.setItem('token', data.token);
        this.$emit('setActiveComp', 'Details');
      } catch (err) {
        this.error = err.message;
        this.password = '';
      }
    },
    
    async handleRegister() {
      try {
        // Form validation
        if (!this.username || !this.email || !this.passwordReg || !this.confirmReg) {
          this.emptyFields = true;
          this.error = "Please fill in all fields";
          return;
        }
        if (!this.email.endsWith('@camp.com')) {
          this.error = "Email must end with @camp.com";
          return;
        }
        if (this.passwordReg !== this.confirmReg) {
          this.error = "Passwords do not match";
          return;
        }
        this.emptyFields = false;
        this.error = ""; // Clear previous error

        // First, create the user in the users collection
        const res = await fetch('http://localhost:3000/users/', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ 
            username: this.username,
            email: this.email, 
            password: this.passwordReg
          }),
        });
        const userData = await res.json();
        if (!res.ok) throw new Error(userData.message || 'Registration failed');

        // After registration succeeds, log in the user
        const loginRes = await fetch('http://localhost:3000/api/auth/signin', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            identifier: this.email, // Use email as identifier for login
            password: this.passwordReg 
          }),
        });
        const loginData = await loginRes.json();
        if (!loginRes.ok) throw new Error(loginData.message || 'Registration successful but login failed');

        // Store the token and redirect
        localStorage.setItem('token', loginData.token);
        this.$emit('setActiveComp', 'Details');
      } catch (err) {
        this.error = err.message;
        this.passwordReg = '';
        this.confirmReg = '';
      }
    },
    
    toggleForm() {
      this.registerActive = !this.registerActive;
      this.emptyFields = false;
      this.error = '';
      
      // Clear all fields
      this.identifier = '';
      this.password = '';
      this.username = '';
      this.email = '';
      this.passwordReg = '';
      this.confirmReg = '';
    }
  },
};
</script>

<template>
  <div class="min-h-screen bg-blue-100 flex items-center justify-center p-8">
    <div class="max-w-lg w-full bg-gray-200 rounded-2xl shadow-2xl p-12">
      <!-- Sign In Form -->
      <div v-if="!registerActive">
        <h2 class="text-4xl font-bold text-gray-900 mb-8 text-center">Sign In</h2>
        
        <form @submit.prevent="handleLogin" class="space-y-6" :class="{ 'border-red-500': emptyFields }">
          <div>
            <label class="block text-lg font-medium text-gray-700 mb-2">Email or Username</label>
            <input 
              type="text" v-model="identifier" placeholder="Enter your email or username"
              class="w-full px-6 py-4 border border-gray-300 rounded-lg" 
              :class="{ 'border-red-500': emptyFields && !identifier }"
            />
          </div>
          <div>
            <label class="block text-lg font-medium text-gray-700 mb-2">Password</label>
            <div class="relative">
              <!--v-model keeps your form fields and your Vue data in sync, 
              making it easy to work with user input.-->
              <input 
                :type="showPassword ? 'text' : 'password'"
                v-model="password" placeholder="*******"
                class="w-full px-6 py-4 border border-gray-300 rounded-lg"
                :class="{ 'border-red-500': emptyFields && !password }"
              />
              <button type="button"
                class="absolute right-4 top-1/2 transform -translate-y-1/2 text-gray-500"
                @click="showPassword = !showPassword"
                tabindex="-1"
              >
                <i :class="showPassword ? 'pi pi-eye-slash' : 'pi pi-eye'"></i>
              </button>
            </div>
          </div>
          <p v-if="error" class="text-red-500">{{ error }}</p>
          <button type="submit" class="w-full bg-green-500 hover:bg-green-600 text-white font-bold py-4 rounded-lg text-xl">
            Sign In
          </button>
          <p class="text-center mt-4">
            Don't have an account? 
            <a @click="toggleForm" href="#" class="text-green-600 hover:text-green-800 font-semibold">
              Sign up here
            </a>
          </p>
        </form>
      </div>
      
      <!-- Sign Up Form -->
      <div v-else>
        <h2 class="text-4xl font-bold text-gray-900 mb-8 text-center">Sign Up</h2>
        
        <form @submit.prevent="handleRegister" class="space-y-6" :class="{ 'border-red-500': emptyFields }">
          <div>
            <label class="block text-lg font-medium text-gray-700 mb-2">Username</label>
            <input 
              type="text" v-model="username" placeholder="Choose a username"
              class="w-full px-6 py-4 border border-gray-300 rounded-lg"
              :class="{ 'border-red-500': emptyFields && !username }"
            />
          </div>
          <div>
            <label class="block text-lg font-medium text-gray-700 mb-2">Email</label>
            <input 
              type="email" v-model="email" placeholder="Enter your email"
              class="w-full px-6 py-4 border border-gray-300 rounded-lg"
              :class="{ 'border-red-500': emptyFields && !email }"
            />
          </div>
          <div>
            <label class="block text-lg font-medium text-gray-700 mb-2">Password</label>
            <div class="relative">
              <input 
                :type="showPasswordReg ? 'text' : 'password'"
                v-model="passwordReg" placeholder="*******"
                class="w-full px-6 py-4 border border-gray-300 rounded-lg"
                :class="{ 'border-red-500': emptyFields && !passwordReg }"
              />
              <button type="button"
                class="absolute right-4 top-1/2 transform -translate-y-1/2 text-gray-500"
                @click="showPasswordReg = !showPasswordReg"
                tabindex="-1"
              >
                <i :class="showPasswordReg ? 'pi pi-eye-slash' : 'pi pi-eye'"></i>
              </button>
            </div>
          </div>
          <div>
            <label class="block text-lg font-medium text-gray-700 mb-2">Confirm Password</label>
            <div class="relative">
              <input 
                :type="showConfirmReg ? 'text' : 'password'"
                v-model="confirmReg" placeholder="*******"
                class="w-full px-6 py-4 border border-gray-300 rounded-lg"
                :class="{ 'border-red-500': emptyFields && !confirmReg }"
              />
              <button type="button"
                class="absolute right-4 top-1/2 transform -translate-y-1/2 text-gray-500"
                @click="showConfirmReg = !showConfirmReg"
                tabindex="-1"
              >
                <i :class="showConfirmReg ? 'pi pi-eye-slash' : 'pi pi-eye'"></i>
              </button>
            </div>
          </div>
          <p v-if="error" class="text-red-500">{{ error }}</p>
          <button type="submit" class="w-full bg-green-500 hover:bg-green-600 text-white font-bold py-4 rounded-lg text-xl">
            Sign Up
          </button>
          <p class="text-center mt-4">
            Already have an account? 
            <a @click="toggleForm" href="#" class="text-green-600 hover:text-green-800 font-semibold">
              Sign in here
            </a>
          </p>
        </form>
      </div>
    </div>
  </div>
</template>