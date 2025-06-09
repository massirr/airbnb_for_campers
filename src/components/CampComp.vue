<script>
import DatePicker from 'vue2-datepicker';
import 'vue2-datepicker/index.css';

export default{
  name: 'CampComp',
  props: {
    camp: Object,
  },
  components: {
    DatePicker
  },
  mounted() {
  },
  data() {
      return {
        bookable: this.camp.bookable,
        showDatePicker: false,    // Controls visibility of the date picker popup
        startDate: null,          
        endDate: null,            
        dateError: ''            
      }
  },
  computed: { //  Computed properties cache their results and only recompute when dependencies change
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
    },
    truncatedDescription() {
      const maxLength = 150; // Set the maximum number of characters
      return this.camp.description.length > maxLength
        ? this.camp.description.substring(0, maxLength) + "..."
        : this.camp.description;
    },
  },
  methods:{
    /**
     * Toggle date picker popup visibility and handle redirection if not logged in
     * When closing popup, reset date selections to start fresh next time
     */
    openDatePicker() {
      if (!this.isLoggedIn) {
        // Redirect to Account/Login page if user is not logged in
        this.$emit('setActivePage', 'Account');
        return;
      }
      // Toggle the visibility of date picker
      this.showDatePicker = !this.showDatePicker;
      
      // Reset dates when closing the popup
      if (!this.showDatePicker) {
        this.startDate = null;
        this.endDate = null;
      }
    },
    
    /**
     * Reset date selections and error message
     * Used when user wants to start over with date selection
     */
    clearDates() {
      this.startDate = null;
      this.endDate = null;
      this.dateError = '';
    },
    
    /**
     * Process booking with selected dates
     * Validates that both dates are selected before proceeding
     * Updates camp status via API and creates booking record
     */
    confirmBooking(spotID) {
      // Validate both dates are selected
      if (!this.startDate || !this.endDate) {
        this.dateError = 'Please select both start and end dates.';
        return;
      }
      
      // Clear any previous error
      this.dateError = '';
      
      // First, save the booking record to the bookings endpoint
      fetch('http://localhost:3000/bookings', {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          userID: this.userId,
          spotID: spotID,
          startDate: this.startDate,  
          endDate: this.endDate,      
          price: this.camp.price
        })
      })
      .then(response => {
        if (!response.ok) {
          throw new Error('Failed to create booking');
        }
        return response.json();
      })
      .then(() => {
        //console.log('Booking created:', bookingData);
        
          // After booking is saved, update the camp spot status
          return fetch(`http://localhost:3000/camps/spots/${spotID}`, {
            method: "POST",
            headers: {
              "Content-Type": "application/json"
            },
            body: JSON.stringify({
              bookable: "false",  // Mark as booked
            })
          });
      })
      .then(response => response.json())
      .then(updatedSpot => {
        // Update local state with server response
        this.bookable = updatedSpot.bookable;
        // Hide the date picker
        this.showDatePicker = false;
        // Reset date selection for next time
        this.startDate = null;
        this.endDate = null;
      })
      .catch(error => {
        console.error("Error processing booking:", error);
        this.dateError = 'Could not complete booking. Please try again.';
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
          {{ `${camp.city}, ${camp.country.substring(0, 3)}` }}
        </div>

        <div class="relative flex flex-col items-start">
          <button
            @click="openDatePicker"
            :disabled="!bookable"
            :class="[
              'h-[36px] text-white px-4 py-2 rounded-lg text-center text-sm',
              bookable === 'true' ? 'bg-green-500 hover:bg-green-600' : 'bg-gray-400 cursor-not-allowed'
            ]"
          >
            {{ bookable === 'true' ? 'Book Camp' : 'Booked' }}
          </button>
          <div v-if="showDatePicker" class="absolute left-0 top-full mt-2 z-50 bg-white p-4 rounded-xl shadow-xl min-w-[260px]">
            <div class="mb-2">
              <label class="block text-sm font-semibold mb-1">{{ !startDate ? 'Select Start Date' : 'Select End Date' }}</label>
              <div v-if="!startDate" class="flex gap-2">
                <DatePicker
                  v-model="startDate"
                  type="date"
                  :not-before="new Date()"
                  placeholder="Start date"
                  class="w-full"
                />
              </div>
              <div v-else class="flex gap-2">
                <DatePicker
                  v-model="endDate"
                  type="date"
                  :not-before="startDate"
                  placeholder="End date"
                  class="w-full"
                />
              </div>
            </div>
            <div v-if="dateError" class="text-red-500 text-xs mb-2">{{ dateError }}</div>
            <div class="flex gap-2 mt-2">
              <button v-if="startDate && endDate" @click="confirmBooking(camp.spotID)" class="bg-green-500 hover:bg-green-600 text-white px-3 py-1 rounded">Confirm</button>
              <button v-if="startDate" @click="clearDates" class="bg-yellow-500 hover:bg-yellow-600 text-white px-3 py-1 rounded">Reset</button>
              <button @click="showDatePicker = false" class="bg-gray-300 hover:bg-gray-400 text-gray-700 px-3 py-1 rounded">Cancel</button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Place this consistently inside the card and remove external margins -->
    <div class="px-4 pb-4">
      <a
        @click="$emit('setActivePage', { page: 'CampInfo', camp })" 
        class="block bg-orange-400 text-white text-center py-1 rounded-xl hover:bg-orange-500"
      ><!--the emit to receive the camp object-->
        More
      </a>
    </div>
  </div>
</template>
