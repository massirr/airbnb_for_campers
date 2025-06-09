<script>
  import Dashboard from './Dashboard.vue';

  // were the logout logic is
  export default {
    name: "DetailsComp",
    data() {
      return {
        user: null,
        activeSection: 'personal', // Track which sidebar section is active
        dashboardData: null // To store data from the dashboard endpoint
      };
    },
    components: {
      Dashboard,
    },
    computed: {
      isLoggedIn() {
        const token = localStorage.getItem('token');
        if (!token) return false;

        try {
          const details = JSON.parse(atob(token.split('.')[1]));
          const currentTime = Math.floor(Date.now() / 1000); // Current time in seconds
          return details.exp > currentTime; // Check if token is still valid
        } catch (e) {
          return false;
        }
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
      },
      isAdmin() {
        const token = localStorage.getItem('token');
        if (!token) return false;

        try {
          const details = JSON.parse(atob(token.split('.')[1]));
          return details.isAdmin === true; // Check if the user is an admin
        } catch (e) {
          return false;
        }
      },
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
      },
      fetchDashboardData() {
        if (!this.isLoggedIn) return;
        const token = localStorage.getItem('token');
        fetch('http://localhost:3000/dashboard', {
          method: 'GET',
          headers: {
            'Authorization': `Bearer ${token}`,
            'Content-Type': 'application/json'
          }
        })
          .then(response => {
            if (!response.ok) throw new Error('Failed to fetch dashboard data');
            return response.json();
          })
          .then(data => {
            this.dashboardData = data;
            //console.log('Dashboard data:', this.dashboardData);
          })
          .catch(error => {
            console.error('Error fetching dashboard data:', error);
          });
      }
    },
    mounted() {
      if (!this.isLoggedIn) {
        localStorage.removeItem('token'); // Ensure token is removed if expired
        this.$emit('setActiveComp', 'SignIn');
        return;
      }
      this.getUser(this.userId); // the userId is returned by the method
      this.fetchDashboardData();
    }
  };
</script>

<template>
  <div v-if="isLoggedIn" class="min-h-screen bg-gray-50 flex flex-col">
    <!-- User profile at the top -->
    <div class="w-full bg-white shadow-md py-8 mb-10">
      <div class="flex flex-col items-center text-center max-w-7xl mx-auto">
        <div class="w-20 h-20 rounded-full bg-orange-300 text-white flex items-center justify-center text-2xl font-bold">
          ID
        </div>
        <h2 class="mt-4 text-xl font-semibold text-gray-800">{{ user?.username || 'Loading...' }}</h2>
        <p class="text-gray-500 text-sm">{{ user?.email || 'Loading...' }}</p>
      </div>
    </div>

    <!-- Main content area -->
    <div class="flex-grow py-4 px-4 flex items-start justify-center">
      <div class="w-full max-w-7xl flex flex-col lg:flex-row gap-10">
        <!-- Sidebar (without the user profile) -->
        <aside class="w-full lg:w-1/4 bg-white rounded-xl shadow p-6">
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
                  >
                    👤 Personal information
                  </button>
                </li>
              </ul>
            </div>
            <div v-if="isAdmin">
              <h3 class="text-sm font-bold text-gray-500 uppercase mb-2">Dashboard</h3>
              <ul class="space-y-2 text-gray-700">
                <li>
                  <button
                    :class="{'text-green-600 font-bold': activeSection === 'dashboard', 'hover:text-green-600': activeSection !== 'dashboard'}" 
                    @click="activeSection = 'dashboard'"
                  >
                    📊 Dashboard
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
          <!-- Rest of your personal info section unchanged -->
          <h1 class="text-4xl font-bold text-gray-800 mb-8 text-center">Personal Information</h1>
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
              <p class="text-gray-600">+32 *********</p>
            </div>
            <div>
              <p class="font-semibold">Birthday</p>
              <p class="text-gray-600">DD / MM / YYYY</p>
            </div>
            <div>
              <p class="font-semibold">Gender</p>
              <p class="text-gray-600">None</p>
            </div>
            <div>
              <p class="font-semibold">Language</p>
              <p class="text-gray-600">None</p>
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

        <!--Dashboard info-->
        <section
          v-if="activeSection === 'dashboard' && isAdmin"
          class="w-full lg:w-3/4 bg-white rounded-xl shadow p-8 flex flex-col items-center"
        >
          <Dashboard :overview="dashboardData"/>
        </section>
      </div>
    </div>
  </div>
</template>