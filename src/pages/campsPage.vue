<script>
  import Camp from '@/components/CampComp.vue';
  import Dropdown from '@/components/DropdownComp.vue';

  export default{
    name: 'campsPage',
    props: {
      limit: Number, // again, from parent to this child
      showButton: { // the view all Camps is set to not show by default
        type: Boolean,
        default: false
      },
      showDropdown: { // the view all Camps is set to not show by default
        type: Boolean,
        default: true
      },
      title:  {
        type: String,
        default: 'Browse Camps'
      }
    },
    data() {
      return { // fetch the camp details
        camps: ['1','2','3','4','5','6']
      }
    },
    components: {
      Camp,
      Dropdown
    }
  }
</script>

<template>
  <section class="bg-blue-100 px-4 py-10">
    <div class="container-xl lg: container m-auto">
      <h2 class="text-3xl font-bold •text-green-500 mb-6 text-center">
        {{title}}
      </h2>
      <Dropdown v-if="showDropdown"/>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <Camp v-for="(camp, index) in camps.slice(0, limit || camps.length)" :key="index" :camp="camp"/> <!--camp is passed to the child as a prop -->
      </div>
    </div> 

    <div v-if="showButton" class="m-auto max-w-lg my-10 px-6">
      <a
        @click="$emit('setActivePage', 'Camps')"
        class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
        >
        View All Camps
      </a>
    </div>
  </section>
</template>