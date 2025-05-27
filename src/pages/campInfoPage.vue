<script>
    import MapComp from '@/components/MapComp.vue';

    export default {
      name: 'compInfoPage',
      components: {
        MapComp,
      },
      props: {
        camp: {
          type: Object,
          required: true
        }
      },
      mounted() {
        //console.log(this.camp); // Log the camp object to the console
      }
    }
</script>

<template>
  <div class="flex flex-col h-screen">
    <!-- Title Section -->
    <div class="w-full text-center p-6 bg-gray-100 shadow-md">
      <h1 class="text-4xl font-bold text-green-700 cursor-pointer" 
          @click="$emit('setActivePage', 'Camps')">
          {{ camp.name }}
      </h1>
    </div>

    <div class="flex flex-col lg:flex-row flex-grow">
      <!-- Left Half -->
      <div class="w-full lg:w-1/2 flex flex-col justify-start p-6">
        <!-- Image Section -->
        <div class="w-full h-[50%] bg-gray-300 rounded-lg shadow-lg mb-4 flex items-center justify-center">
          <img v-if="camp.campingSpot_images && camp.campingSpot_images.length"
           :src="camp.campingSpot_images[0].imageURL" 
           alt="Camp Image" 
           class="w-full h-full object-cover rounded-lg" />
          <p v-else class="text-gray-500">Add an image here</p>
        </div>
        
        <!-- MapComp Section -->
        <div class="flex-grow">
          <MapComp />
        </div>
      </div>

      <!-- Right Half -->
      <div class="w-full lg:w-1/2 flex flex-col p-6">
        <!-- Features Section -->
        <h2 class="text-xl font-bold mb-2 text-center underline">Features</h2>
        <div class="grid grid-cols-2 gap-2 mb-4">
          <div v-for="(feature, index) in camp.campingSpot_features" :key="index" class="bg-gray-100 p-2 rounded-lg shadow-md flex items-center">
            <i class="pi pi-star text-yellow-500 mr-2"></i>
            <p class="text-gray-700 font-bold">{{ feature.features.featureName }}</p>
          </div>
        </div>
        <!-- Description Section -->
        <h2 class="text-xl font-bold mb-2 text-center underline">Description</h2>
        <div class="bg-gray-100 p-6">
          <p class="text-lg text-gray-800 leading-relaxed">{{ camp.description }}</p>
        </div>
      </div>
    </div>
  </div>
</template>