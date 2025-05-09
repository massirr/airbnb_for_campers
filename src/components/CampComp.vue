<script>

  export default{
    name: 'CampComp',
    props: {
      camp: Object,
    },
    mounted() {
    },
    data() {
        return {
          bookable: this.camp.bookable 
        }
    },
    computed: {
      truncatedDescription() {
        const maxLength = 150; // Set the maximum number of characters
        return this.camp.description.length > maxLength
          ? this.camp.description.substring(0, maxLength) + "..."
          : this.camp.description;
      },
    },
    methods:{
      bookCamp(spotID) {
        fetch(`http://localhost:3000/camps/spots/${spotID}`, {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify({ bookable: "false" }) // Send bookable as a string like in postman
        })
          .then((response) => response.json())
          .then((updatedSpot) => {
            this.bookable = updatedSpot.bookable; // update the local state to reflect the change
          })
          .catch((error) => {
            console.error("Error updating camp status:", error);
          });
      }
    },
  }

</script>

<template>
  <div class="bg-white rounded-xl shadow-md relative flex flex-col justify-between h-full">
    <div class="p-4">
      <div class="mb-6">
        <h3 class="text-xl font-bold">{{ camp.name }}</h3>
      </div>

      <div class="mb-5">
        {{ truncatedDescription }}
      </div>

      <h3 class="text-green-400 mb-2">${{ camp.price }}</h3>

      <div class="border border-gray-100 mb-5"></div>

      <div class="flex flex-col lg:flex-row justify-between mb-4">
        <div class="text-orange-700 mb-3">
          <i class="pi pi-map-marker text-orange-700"></i>
          {{ `${camp.city}, Bel` }}
        </div>

        <button
          @click="bookCamp(camp.spotID)"
          :disabled="!bookable"
          :class="[
            'h-[36px] text-white px-4 py-2 rounded-lg text-center text-sm',
            bookable === 'true' ? 'bg-green-500 hover:bg-green-600' : 'bg-gray-400 cursor-not-allowed'
          ]"
        >
          {{ bookable === 'true' ? 'Book Camp' : 'Booked' }}
        </button>
      </div>
    </div>

    <!-- Place this consistently inside the card and remove external margins -->
    <div class="px-4 pb-4">
      <a
        @click="$emit('setActivePage', 'CampInfo')"
        class="block bg-orange-400 text-white text-center py-1 rounded-xl hover:bg-orange-500"
      >
        More
      </a>
    </div>
  </div>
</template>
