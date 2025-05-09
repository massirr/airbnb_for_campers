<script>
    export default {
      name: "bookingsPage",
      data() {
        return {
          bookedCamps: null
        }
      },
      mounted() {
        this.getCamps()
      },
      methods: {
        getCamps() {
        fetch("http://localhost:3000/camps/spots?bookable=false", {
          method: "GET"
        })
            .then((response) => response.json())
            .then((_camps) => {
              this.bookedCamps = _camps
              //console.log(this.bookedCamps)
            })
            .catch((error) => {
              console.error("Error fetching camps:", error); // Handle errors
            });
        }
      }
    };
</script>

<template>
  <div class="min-h-screen bg-blue-100 flex flex-col items-center p-6">
    <h2 class="text-3xl font-bold text-green-500 mb-8 text-center">
      Your Booked Camps
    </h2>
    <div
      v-if="bookedCamps && bookedCamps.length > 0"
      class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 w-full max-w-6xl"
    >
      <div
        v-for="(camp, index) in bookedCamps"
        :key="index"
        class="bg-white rounded-xl shadow-md p-6 flex flex-col justify-between"
      >
        <h3 class="text-xl font-bold text-gray-900 mb-4">{{ camp.name }}</h3>
        <p class="text-gray-700 mb-2">
          <span class="font-semibold">Price:</span> ${{ camp.price }}
        </p>
        <p class="text-gray-700 mb-4">
          <span class="font-semibold">Location:</span> {{ camp.city }}, Bel
        </p>
        <button
          class="bg-red-500 hover:bg-red-600 text-white font-medium py-2 px-4 rounded-lg transition-colors"
        >
          Cancel Booking
        </button>
      </div>
    </div>
    <div
      v-else
      class="text-center text-gray-600 text-lg mt-10"
    >
      You have no booked camps yet.
    </div>
  </div>
</template>