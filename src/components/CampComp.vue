<script>

  export default{
    name: 'CampComp',
    props: {
      camp: String,
    }
  }

</script>

<template>
    <div class="bg-white rounded-xl shadow-md relative">
      <div class="p-4">
        <div class="mb-6">
          <div class="text-gray-600 my-2">{{camp}}</div>
            <h3 class="text-xl font-bold">Front-End Engineer (Vue)</h3>
        </div>

        <div class="mb-5">
          Join our team as a Front-End Developer in sunny Miami, FL. We
          are looking for a motivated individual with a passion...
        </div>

        <h3 class="text-green-500 mb-2">$70K - $80K / Year</h3>

        <div class="border border-gray-100 mb-5"></div>

        <div class="flex flex-col lg:flex-row justify-between mb-4">
          <div class="text-orange-700 mb-3">
            <i class="pi pi-map-marker text-orange-700"></i>
              Miami, FL
          </div>
          <button
            href="job.html"
            class="h-[36px] bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg text-center text-sm"
          >
            Book Camp
          </button>
        </div>
      </div>
    </div>
</template>