<script>
  import Camp from '@/components/CampComp.vue';
  import Dropdown from '@/components/DropdownComp.vue';

  export default{
    name: 'campsPage',
    props: {
      limit: Number, // again, from parent to this child, the number of camps to show
      showButton: { // the view all Camps is set to not show by default
        type: Boolean,
        default: false
      },
      showDropdown: { // the dropdown is set to show by default
        type: Boolean,
        default: true
      },
      title:  {
        type: String,
        default: 'Browse Camps'
      },
      bookedState: Array
    },
    mounted() {
      this.getCamps()
    },
    data() {
      return { // fetched camp details
        camps: null,
        camp_features: [],
        selectedFeatures: [] // List of currently selected features
      }
    },
    components: {
      Camp,
      Dropdown
    },
    methods: {
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
      handleFeatureSelected(featureName, checked) {
        if (!this.camps) return;

        if (checked) {
          // Add the selected feature to the list
          this.selectedFeatures.push(featureName);
        } else {
          // Remove the deselected feature from the list
          this.selectedFeatures = this.selectedFeatures.filter(
            (feature) => feature !== featureName
          );
        }

        // Filter camps based on all selected features
        if (this.selectedFeatures.length > 0) {
          this.camp_features = this.camps.filter((camp) =>
            this.selectedFeatures.every((selectedFeature) =>
              camp.campingSpot_features &&
              camp.campingSpot_features.some(
                (feature) => feature.features.featureName === selectedFeature
              )
            )
          );
        } else {
          // If no features are selected, reset camp_features
          this.camp_features = [];
        }

        //console.log("Selected Features:", this.selectedFeatures);
        //console.log("Filtered Camps:", this.camp_features);
      }
    }
  }
</script>

<template>
  <section class="bg-blue-100 px-4 py-10">
    <div class="container-xl lg: container m-auto">
      <h2 class="text-3xl font-bold •text-green-500 mb-6 text-center">
        {{title}}
      </h2>
      <div class="relative flex flex-col items-center">
        <Dropdown v-if="showDropdown" class="absolute z-20 top-0 left-1/2 -translate-x-1/2" 
                  @feature-selected="handleFeatureSelected" />
      </div>
      <div v-if="camp_features.length > 0" class="grid grid-cols-1 md:grid-cols-3 gap-6 mt-16">
        <!-- Render Camp components based on filtered camps -->
        <Camp v-for="(_camp, index) in camp_features.slice(0, limit || camp_features.length)" 
              :key="index"
              :camp="_camp" 
              @setActivePage="$emit('setActivePage', $event)"/>
      </div>
      <div v-else-if="camp_features.length === 0 && selectedFeatures.length > 0" 
            class="flex items-center justify-center h-[50vh] text-center text-gray-500">
        <!-- Show message when no camps match the selected features -->
        <p class="text-xl font-semibold text-gray-700 bg-gray-100 p-6 rounded-lg shadow-md">
          No camps match the selected features.
        </p>
      </div>
      <div v-else-if="camps" class="grid grid-cols-1 md:grid-cols-3 gap-6 mt-16">
        <!-- Render Camp components based on all camps if no features are selected -->
        <Camp v-for="(_camp, index) in camps.slice(0, limit || camps.length)" 
              :key="index"
              :camp="_camp" 
              @setActivePage="$emit('setActivePage', $event)"/>
      </div>
    </div> 

    <div v-if="showButton" class="flex justify-center my-10">
      <button
        @click="$emit('setActivePage', 'Camps')"
        class="block bg-black text-white text-center py-4 w-full max-w-xl rounded-xl hover:bg-gray-700"
      >
        View All Camps
      </button>
    </div>
  </section>
</template>