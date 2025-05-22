<script>
  export default {
    name: "DetailsComp",
    data() {
      return {
        user: null,
        activeSection: 'personal' // Track which sidebar section is active
      };
    },
    computed: {
      isLoggedIn() {
        return !!localStorage.getItem('token');
      },
      userId() {
        const token = localStorage.getItem('token');
        if (!token) return null;
        try {
          const details = JSON.parse(atob(token.split('.')[1]));
          return details.userId || null;
        } catch (e) {
          return null;
        }
      }
    },
    methods: {
      logout() {
        localStorage.removeItem('token');
        this.$emit('setActiveComp', 'SignIn');
      },
      getUser(userId) {
        if (!userId) return;
        fetch(`http://localhost:3000/users/${userId}`)
          .then((response) => {
            if (!response.ok) throw new Error('User not found');
            return response.json();
          })
          .then((_user) => {
            this.user = _user;
            //console.log(this.user.username);
          })
          .catch((error) => {
            this.user = null;
            console.error('Error fetching user:', error);
          });
      }
    },
    mounted() {
      if (!this.isLoggedIn) {
        this.$emit('setActiveComp', 'SignIn');
        return;
      }
      this.getUser(this.userId); // the userId is returned by the method
    }
  };
</script>

<template>
  <div v-if="isLoggedIn" class="min-h-screen bg-gray-50 py-10 px-4 flex items-start justify-center">
    <div class="w-full max-w-7xl flex flex-col lg:flex-row gap-10">

      <!-- Sidebar -->
      <aside class="w-full lg:w-1/4 bg-white rounded-xl shadow p-6">
        <div class="flex flex-col items-center text-center mb-10">
          <div class="w-20 h-20 rounded-full bg-orange-300 text-white flex items-center justify-center text-2xl font-bold">
            ID
          </div>
          <h2 class="mt-4 text-xl font-semibold text-gray-800">{{ user?.username || 'Loading...' }}</h2>
          <p class="text-gray-500 text-sm">{{ user?.email || 'Loading...' }}</p>
        </div>

        <nav class="space-y-8 text-left">
          <div>
            <h3 class="text-sm font-bold text-gray-500 uppercase mb-2">Your activity</h3>
            <ul class="space-y-2 text-gray-700">
              <li>
                <button
                  :class="{'text-green-600 font-bold': activeSection === 'bookings', 'hover:text-green-600': activeSection !== 'bookings'}"
                  @click="activeSection = 'bookings'"
                >
                  📅 Bookings
                </button>
              </li>
            </ul>
          </div>
          <div>
            <h3 class="text-sm font-bold text-gray-500 uppercase mb-2">Account settings</h3>
            <ul class="space-y-2 text-gray-700">
              <li>
                <button
                  :class="{'text-orange-500 font-bold': activeSection === 'personal', 'hover:text-orange-500': activeSection !== 'personal'}" 
                  @click="activeSection = 'personal'"
                > <!--Binded the class to use javascript, if, else statement-->
                  👤 Personal information
                </button>
              </li>
            </ul>
          </div>
        </nav>
      </aside>

      <!-- Personal Info -->
      <section
        v-if="activeSection === 'personal'"
        class="w-full lg:w-3/4 bg-white rounded-xl shadow p-8 space-y-6 flex flex-col items-center justify-center"
      >
        <h1 class="text-2xl font-bold text-gray-800 mb-6 text-center">Personal information</h1>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 text-gray-800 w-full max-w-2xl mx-auto">
          <div>
            <p class="font-semibold">First name</p>
            <p class="text-gray-600">{{ user?.username || 'N/A' }}</p>
          </div>
          <div>
            <p class="font-semibold">Last name</p>
            <p class="text-gray-600">{{ user?.username || 'N/A' }}</p>
          </div>
          <div>
            <p class="font-semibold">Email address</p>
            <p class="text-gray-600">{{ user?.email || 'N/A' }}</p>
          </div>
          <div>
            <p class="font-semibold">Mobile number</p>
            <p class="text-gray-600">+32 465329016</p>
          </div>
          <div>
            <p class="font-semibold">Birthday</p>
            <p class="text-gray-600">DD / MM / YYYY</p>
          </div>
          <div>
            <p class="font-semibold">Gender</p>
            <p class="text-gray-600">Male</p>
          </div>
          <div>
            <p class="font-semibold">Language</p>
            <p class="text-gray-600">English</p>
          </div>
        </div>
        <div class="mt-15 text-right w-full max-w-2xl mx-auto">
          <button @click="logout" class="text-red-500 hover:text-red-600 text-lg font-semibold">
            Logout
          </button>
        </div>
      </section>

      <!--Bookings info-->
      <section
        v-if="activeSection === 'bookings'"
        class="w-full lg:w-3/4 bg-white rounded-xl shadow p-8 flex flex-col items-center justify-center min-h-[300px]"
      >
        <h1 class="text-2xl font-bold text-gray-800 mb-6 text-center">Bookings</h1>
        <p class="text-lg text-gray-600 text-center">This is the page with the booking info.</p>
      </section>
    </div>
  </div>
</template>