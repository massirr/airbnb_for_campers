<script>
    export default {
      name: "bookingsPage",
      props: {
        bookedItems: Array,
        cancelBooking: {
          type: Boolean,
          default: false
        }
      },
      mounted() {
        if (this.bookedItems) {
          this.bookedItems.forEach((item) => {
            this.changeState(item.name);
          });
        }
      },
      methods: {
        changeState(name) {
          if (!this.cancelBooking) {
            this.$emit("bookedState", name); // Emit the bookedState event
          } else {
            // Emit an event to the parent to remove the item
            console.log(name);
          }
        },
      },
    };
</script>

<template>
  <div class="min-h-screen bg-blue-100 flex flex-col items-center p-6">
    <h2 class="text-3xl font-bold text-green-500 mb-8 text-center">
      Your Booked Camps
    </h2>
    <div
      v-if="bookedItems && bookedItems.length > 0"
      class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 w-full max-w-6xl"
    >
      <div
        v-for="(item, index) in bookedItems"
        :key="index"
        class="bg-white rounded-xl shadow-md p-6 flex flex-col justify-between"
      >
        <h3 class="text-xl font-bold text-gray-900 mb-4">{{ item.name }}</h3>
        <p class="text-gray-700 mb-2">
          <span class="font-semibold">Price:</span> ${{ item.price }}
        </p>
        <p class="text-gray-700 mb-4">
          <span class="font-semibold">Location:</span> {{ item.city }}, Bel
        </p>
        <button
          @click="changeState(item.name)"
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