<script>

  export default{
    name: 'CampComp',
    props: {
      camp: Object,
      bookedState: Array, // we expect an array of items
    },
    mounted() {
      // Check if the camp is already booked when the component is mounted
      this.checkIfBooked();
    },
    data() {
        return {
          bookedCamps: null,
          isBooked: false  // track state of Book Camp button
        }
    },
    methods:{
      checkIfBooked() {
        // Check if the current camp is in the bookedState array return boolean
        if(this.bookedState) {
          const booked = this.bookedState.some(
            (item) => item === this.camp //  objects have items(the name)
          );
          this.isBooked = booked;          
        }
      },
      bookCamp(camp) {
        this.$emit("booked", camp); // from Camp to camps to app then binded to bookings
        this.isBooked = true
      }
    }
  }

</script>

<template>
    <div class="bg-white rounded-xl shadow-md relative">
      <div class="p-4">
        <div class="mb-6">
            <h3 class="text-xl font-bold">{{camp.name}}</h3>
        </div>

        <div class="mb-5">
          {{camp.description}}
        </div>

        <h3 class="text-green-400 mb-2">${{camp.price}}</h3>

        <div class="border border-gray-100 mb-5"></div>

        <div class="flex flex-col lg:flex-row justify-between mb-4">
          <div class="text-orange-700 mb-3">
            <i class="pi pi-map-marker text-orange-700"></i>
              Miami, FL
          </div>
          <button
            @click="bookCamp(camp)"
            :disabled="isBooked"
            :class="[
              'h-[36px] text-white px-4 py-2 rounded-lg text-center text-sm',
              isBooked ? 'bg-gray-400 cursor-not-allowed' : 'bg-green-500 hover:bg-green-600'
            ]"
          >
            {{ isBooked ? 'Booked' : 'Book Camp' }} <!--if true show Booked else you can Book-->
          </button>
        </div>
      </div>
    </div>
</template>