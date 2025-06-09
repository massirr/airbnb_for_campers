<script>
    import DropDown from '@/components/DropdownComp.vue'

    export default {
      name: 'createCampPage',
      components: {
        DropDown
      },
      data() {
        return {
          sighned: false,
          camp: {
            name: '',
            description: '',
            price: '',
            latitude: '',
            longitude: '',
            country: '',
            city: '',
            capacity: '',
            features: [],
          },
          dropDownKey: 0, // Key to force re-render of DropDown
        };
      },
      mounted() {
        const token = localStorage.getItem('token');
        if (token) {
          this.sighned = true;
        }
      },
      methods: {
        async submitCamp() {
          // Initialize an array to store missing fields
          const missingFields = [];

          // Check each field and add its name to the array if it's empty
          if (!this.camp.name) missingFields.push("Name");
          if (!this.camp.description) missingFields.push("Description");
          if (!this.camp.price) missingFields.push("Price");
          if (!this.camp.country) missingFields.push("Country");
          if (!this.camp.city) missingFields.push("City");
          if (!this.camp.capacity) missingFields.push("Capacity");
          if (this.camp.features.length === 0) missingFields.push("Features");

          // If there are missing fields, show an alert and stop submission
          if (missingFields.length > 0) {
            alert(`Please fill in the following fields: ${missingFields.join(", ")}`);
            return;
          }
          
          // Fetch latitude and longitude using city and country
          await this.getLatLon(this.camp.city, this.camp.country);

          // Post the camp data
          await this.createCamp();
          console.log(this.camp);

          // Reset the form after submission
          this.camp = {
            name: '',
            description: '',
            price: '',
            latitude: '', // fetched from the API using the city, country combination
            longitude: '',
            country: '',
            city: '',
            capacity: '',
            features: [],
          };

          // Increment the key to reset the DropDown component
          this.dropDownKey++;
        },
        handleFeatureSelected(featureID, checked) {
          if (checked) {
            //console.log(featureID, checked)
            // Add the selected feature to the list
            this.camp.features.push(featureID);
          }
        },
        async getLatLon(city, country) {
          if (city && country) {
            try {
              const response = await fetch(`http://localhost:3000/latlon/?location=${city},${country}`, {
                method: "GET",
              });
              const _latlon = await response.json();
              // Update latitude and longitude in the camp object
              this.camp.latitude = _latlon.lat;
              this.camp.longitude = _latlon.lon;
              //console.log("Latitude and Longitude fetched:", _latlon);
            } catch (error) {
              console.error("Error fetching latitude and longitude:", error);
            }
          }
        },
        async createCamp() {
          try {
            const response = await fetch('http://localhost:3000/camps/', {
              method: "POST",
              headers: {
                "Content-Type": "application/json",
              },
              body: JSON.stringify(this.camp),
            });

            if (!response.ok) {
              throw new Error("Failed to create camp");
            }

            const result = await response.json();
            alert("Camp created successfully!");
            console.log("Camp created successfully:", result);
          } catch (error) {
            console.error("Error creating camp:", error);
            alert("An error occurred while creating the camp. Please try again.");
          }
        },
      },
    };
</script>

<template>
  <div class="p-6 bg-gray-100 min-h-screen flex items-center justify-center">
    <p v-if="!sighned" class="text-2xl text-gray-800 font-bold leading-relaxed bg-white p-8 rounded-lg shadow-lg text-center max-w-3xl">
      Creating a camp allows you to share your unique outdoor experiences with others. Whether it's a serene lakeside retreat or a thrilling mountain adventure,
       your camp can become a haven for nature enthusiasts seeking to escape the hustle and bustle of daily life. 
      <span @click="$emit('setActivePage', 'Account')" class="text-green-500 cursor-pointer hover:underline">Join</span> 
      our community of camp creators and inspire others to explore the beauty of the great outdoors.
    </p>
    <div v-else class="bg-white p-8 rounded-lg shadow-lg max-w-3xl w-full">
      <h2 class="text-2xl text-gray-800 font-bold mb-6 text-center">Create a Camp</h2>
      <form @submit.prevent>
        <div class="mb-4">
          <label class="block text-gray-700 font-bold mb-2">Name</label>
          <input v-model="camp.name" type="text" class="w-full p-2 border border-gray-300 rounded-lg" placeholder="Enter camp name" />
        </div>
        <div class="mb-4">
          <label class="block text-gray-700 font-bold mb-2">Description</label>
          <textarea v-model="camp.description" class="w-full p-2 border border-gray-300 rounded-lg" placeholder="Enter camp description"></textarea>
        </div>
        <div class="mb-4">
          <label class="block text-gray-700 font-bold mb-2">Price (€)</label>
          <input v-model="camp.price" type="number" step="0.01" min="0" class="w-full p-2 border border-gray-300 rounded-lg" placeholder="Enter price" />
        </div>
        <div class="mb-4">
          <label class="block text-gray-700 font-bold mb-2">Country</label>
          <input v-model="camp.country" type="text" class="w-full p-2 border border-gray-300 rounded-lg" placeholder="Enter country" />
        </div>
        <div class="mb-4">
          <label class="block text-gray-700 font-bold mb-2">City</label>
          <input v-model="camp.city" type="text" class="w-full p-2 border border-gray-300 rounded-lg" placeholder="Enter city" />
        </div>
        <div class="mb-4">
          <label class="block text-gray-700 font-bold mb-2">Capacity</label>
          <input v-model="camp.capacity" type="number" min="0" class="w-full p-2 border border-gray-300 rounded-lg" placeholder="Enter capacity" />
        </div>
        <div class="mb-4">
          <label class="block text-gray-700 font-bold mb-2">Features</label>
          <DropDown
            @feature-selected="handleFeatureSelected" 
            :key="dropDownKey" :emitFeatureID="true"
          /> 
          <!--:key forces Vue to re-render the DropDown component whenever the dropDownKey changes.-->
        </div>
        <button 
          @click="submitCamp"
          type="button"
          class="w-full bg-green-500 text-white p-2 rounded-lg hover:bg-green-600">
          Create Camp
        </button> <!--type="button" prevents the form's default submission behavior.-->
      </form>
    </div>
  </div>
</template>