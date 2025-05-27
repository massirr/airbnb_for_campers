<script>
    export default {
      name: "bookingsPage",
      data() {
        return {
          bookedCamps: null,
        }
      },
      mounted() {
        this.getCamps()
      },
      methods: {
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
        getCamps() { // the bookings that are not canceled
          fetch(`http://localhost:3000/bookings/${this.userId()}`, {
            method: "GET"
          })
              .then((response) => response.json())
              .then((_camps) => {
                //console.log("Raw API response:", _camps);
                
                // Check if _camps is array, if not, set to empty array
                if (!Array.isArray(_camps)) {
                  console.error("API did not return an array:", _camps);
                  this.bookedCamps = [];
                  return;
                }
                
                // Use a Set to track unique spotIDs
                const uniqueSpotIDs = new Set();
                
                // Filter out duplicates based on spotID
                this.bookedCamps = _camps.filter(camp => {
                  // Skip invalid camps
                  if (!camp || !camp.campingSpot_bookings || !camp.campingSpot_bookings[0]) return false;
                  
                  const spotID = camp.campingSpot_bookings[0].spotID;
                  // If we've seen this ID before, skip it
                  if (uniqueSpotIDs.has(spotID)) return false;
                  
                  // Otherwise add it to our Set and keep this item
                  uniqueSpotIDs.add(spotID);
                  return true;
                });
                
                //console.log("Unique camps:", this.bookedCamps);
              })
              .catch((error) => {
                console.error("Error fetching camps:", error); // Handle errors
              });
        },
        cancelBooking(spotID, bookingID) {
          // First, mark the booking as canceled
          fetch(`http://localhost:3000/bookings/${bookingID}`, {
            method: "POST",
            headers: {
              "Content-Type": "application/json"
            },
            body: JSON.stringify({ isCanceled: "true" }) // Mark booking as canceled
          })
            .then(response => {
              if (!response.ok) {
                throw new Error('Failed to update booking status');
              }
              return response.json();
            })
            .then(() => {
              //console.log('Booking marked as canceled:', updatedBooking);
              
              // Then update the camp spot to be bookable again
              return fetch(`http://localhost:3000/camps/spots/${spotID}`, {
                method: "POST",
                headers: {
                  "Content-Type": "application/json"
                },
                body: JSON.stringify({ bookable: "true" }) // Mark spot as available
              });
            })
            .then(response => response.json())
            .then(() => {
              //console.log('Camp spot marked as available:', updatedSpot);
              
              // Refresh the camps list to reflect changes
              this.getCamps();
            })
            .catch((error) => {
              console.error("Error during cancellation process:", error);
            });
        },
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
        <h3 class="text-xl font-bold text-gray-900 mb-4">{{ camp.campingSpot_bookings[0].campingSpot.name }}</h3>
        <p class="text-gray-700 mb-2">
          <span class="font-semibold">Price:</span> ${{ camp.campingSpot_bookings[0].campingSpot.price }}
        </p>
        <p class="text-gray-700 mb-4">
          <span class="font-semibold">Location:</span> {{ camp.campingSpot_bookings[0].campingSpot.city }}
        </p>
        <p class="text-gray-700 mb-4">
          <span class="font-semibold">Start Date:</span> {{ camp.startDate.split('T')[0] }}
        </p>
        <p class="text-gray-700 mb-4">
          <span class="font-semibold">End Date:</span> {{ camp.endDate.split('T')[0] }}
        </p>
        <button
          @click="cancelBooking(camp.campingSpot_bookings[0].spotID, camp.bookingID)"
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