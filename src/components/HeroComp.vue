<script>

  export default {
    name: "HeroComp",
    data() {
      return {
      }
    }
  };
</script>

<template>
  <section class="relative bg-cover bg-center py-24 mb-4">
    <div class="max-w-6xl mx-auto px-6 text-center">
      <!-- Title -->
      <h1 class="text-4xl sm:text-5xl md:text-6xl font-extrabold text-gray-900 leading-tight">
        Discover the Joy of Camping
      </h1>

      <!-- Subtitle -->
      <p class="mt-6 text-lg sm:text-xl md:text-2xl text-gray-700 max-w-3xl mx-auto">
        Escape the hustle and bustle of daily life and reconnect with nature. Explore the best camping spots tailored just for you.
      </p>

      <!-- Buttons -->
      <div class="mt-10 flex justify-center flex-wrap gap-4">
        <button
          @click="$emit('setCampPage', 'Camps')"
          class="bg-green-600 hover:bg-green-700 text-white font-semibold py-3 px-8 rounded-xl shadow-lg transition-transform transform hover:scale-105"
        >
          Explore Camps
        </button>
        <button
          @click="$emit('setCreatePage', 'Camps')"
          class="bg-white hover:bg-gray-100 text-gray-800 border border-gray-300 font-semibold py-3 px-8 rounded-xl shadow-sm transition-transform transform hover:scale-105"
        >
          Learn More
        </button>
      </div>
    </div>
  </section>
</template>
