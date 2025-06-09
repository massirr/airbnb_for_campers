<script>

  export default {
    name: "DashboardComp",
    data() {
      return {
        users: null,
        camps: null,
        bookings: null,
      }
    },
    mounted() {
      this.getUsers()
      this.getCamps()
      this.getBookings()
    },
    methods: {
      getUsers() {
        fetch("http://localhost:3000/users/", {
          method: "GET"
        })
            .then((response) => response.json())
            .then((_users) => {
              this.users = _users
              //console.log(this.users)
            })
            .catch((error) => {
              console.error("Error fetching camps:", error); // Handle errors
            });
      },
      getCamps() {
        fetch("http://localhost:3000/camps", {
          method: "GET"
        })
            .then((response) => response.json())
            .then((_camps) => {
              this.camps = _camps
              //console.log(this.camps)
            })
            .catch((error) => {
              console.error("Error fetching camps:", error); // Handle errors
            });
      },
      getBookings() {
        fetch("http://localhost:3000/bookings", {
          method: "GET"
        })
            .then((response) => response.json())
            .then((_bookings) => {
              this.bookings = _bookings
              //console.log(this.camps)
            })
            .catch((error) => {
              console.error("Error fetching camps:", error); // Handle errors
            });
      },
      truncatedText(text) {
        const maxLength = 30; // Set the maximum number of characters
        return text.length > maxLength
          ? text.substring(0, maxLength) + "..." //substring uses indices
          : text;
      },
    }
  };
</script>

<template>
  <div class="bg-gray-100 min-h-screen p-6">
    <h1 class="text-4xl font-bold text-gray-800 mb-8 text-center">Dashboard</h1>

    <!-- Users Section -->
    <section class="mb-12">
      <h2 class="text-2xl font-semibold text-gray-700 mb-4">Users</h2>
      <div class="overflow-x-auto bg-white shadow-md rounded-lg">
        <table class="table-auto border-collapse border border-gray-300 w-full text-left">
          <thead class="bg-blue-500 text-white">
            <tr>
              <th class="border border-gray-300 px-4 py-2" v-for="(key, keyIndex) in Object.keys(users[0] || {})" :key="keyIndex">
                {{ key }}
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(user, index) in users" :key="index" class="hover:bg-blue-100">
              <td class="border border-gray-300 px-4 py-2" v-for="(value, keyIndex) in Object.values(user)" :key="keyIndex">
                {{ truncatedText(value) }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>

    <!-- Camps Section -->
    <section class="mb-12">
      <h2 class="text-2xl font-semibold text-gray-700 mb-4">Camps</h2>
      <div class="overflow-x-auto bg-white shadow-md rounded-lg">
        <table class="table-auto border-collapse border border-gray-300 w-full text-left">
          <thead class="bg-green-500 text-white">
            <tr>
              <th class="border border-gray-300 px-4 py-2">Name</th>
              <th class="border border-gray-300 px-4 py-2">Description</th>
              <th class="border border-gray-300 px-4 py-2">Price (€)</th>
              <th class="border border-gray-300 px-4 py-2">Location</th>
              <th class="border border-gray-300 px-4 py-2">Capacity</th>
              <th class="border border-gray-300 px-4 py-2">Images</th>
              <th class="border border-gray-300 px-9 py-2">Features</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(camp, index) in camps" :key="index" class="hover:bg-green-100">
              <td class="border border-gray-300 px-4 py-2">{{ camp.name }}</td>
              <td class="border border-gray-300 px-4 py-2">{{ truncatedText(camp.description) }}</td>
              <td class="border border-gray-300 px-4 py-2">{{ camp.price }}</td>
              <td class="border border-gray-300 px-4 py-2">{{ camp.city }}, {{ camp.country }}</td>
              <td class="border border-gray-300 px-4 py-2">{{ camp.capacity }}</td>
              <td class="border border-gray-300 px-4 py-2">
                <div class="flex flex-wrap gap-2">
                  <img v-for="(image, imgIndex) in camp.campingSpot_images" :key="imgIndex" :src="image.imageURL" alt="Camp Image" class="w-16 h-16 object-cover rounded-md">
                </div>
              </td>
              <td class="border border-gray-300 px-4 py-2">
                <ul>
                  <li v-for="(feature, featIndex) in camp.campingSpot_features" :key="featIndex">
                    <i class="pi pi-star text-yellow-500 mr-0 text-xs"></i>
                    {{ feature.features.featureName }}
                  </li>
                </ul>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>

    <!-- Bookings Section -->
    <section class="mb-12">
      <h2 class="text-2xl font-semibold text-gray-700 mb-4">Bookings</h2>
      <div class="overflow-x-auto bg-white shadow-md rounded-lg">
        <table class="table-auto border-collapse border border-gray-300 w-full text-left">
          <thead class="bg-red-500 text-white">
            <tr>
              <th class="border border-gray-300 px-4 py-2" v-for="(key, keyIndex) in Object.keys(bookings[0] || {})" :key="keyIndex">
                {{ key }}
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(booking, index) in bookings" :key="index" class="hover:bg-red-100">
              <td class="border border-gray-300 px-4 py-2" v-for="(value, keyIndex) in Object.values(booking)" :key="keyIndex">
                {{ truncatedText(value) }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>
  </div>
</template>
