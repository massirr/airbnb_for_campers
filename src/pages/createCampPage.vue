<script>
  export default {
    name: 'createCampPage',
    data() {
      return {
        sighned: false,
      };
    },
    mounted() {
      const token = localStorage.getItem('token');
      if (token) {
        this.sighned = true;
      }
    },
  };
</script>

<template>
  <div class="p-6 bg-gray-100 min-h-screen flex items-center justify-center">
    <p v-if="!sighned" class="text-2xl text-gray-800 font-bold leading-relaxed bg-white p-8 rounded-lg shadow-lg text-center max-w-3xl">
      Creating a camp allows you to share your unique outdoor experiences with others. Whether it's a serene lakeside retreat or a thrilling mountain adventure, your camp can become a haven for nature enthusiasts seeking to escape the hustle and bustle of daily life. 
      <span @click="$emit('setActivePage', 'Account')" class="text-green-500 cursor-pointer hover:underline">Join</span> our community of camp creators and inspire others to explore the beauty of the great outdoors.
    </p>
    <p v-else class="text-2xl text-gray-800 font-bold leading-relaxed bg-white p-8 rounded-lg shadow-lg text-center max-w-3xl">
      How to Create a Camp
    </p>
  </div>
</template>