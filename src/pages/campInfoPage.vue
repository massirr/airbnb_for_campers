<script>
    import MapComp from '@/components/MapComp.vue';

    export default {
      name: 'compInfoPage',
      components: {
        MapComp,
      }
    }
</script>

<template>
  <div class="flex flex-col lg:flex-row h-screen">
    <!-- Left Half -->
    <div class="w-full lg:w-1/2 flex flex-col justify-between p-6">
      <!-- Image Section -->
      <div class="w-full h-[50%] bg-gray-300 rounded-lg shadow-lg mb-4 flex items-center justify-center">
        <p class="text-gray-500">Add an image here</p>
      </div>

      <!-- MapComp Section -->
      <div >
        <MapComp />
      </div>
    </div>

    <!-- Right Half -->
    <div class="w-full lg:w-1/2 bg-gray-100 p-6">
      <h1 class="text-3xl text-center font-bold mb-4">Camp Information</h1>
      <p class="text-lg text-gray-700">
        Add your camp details or any other content here. This section will take up the right half of the page.
      </p>
    </div>
  </div>
</template>