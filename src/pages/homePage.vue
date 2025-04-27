<script setup>
    import Hero from "@/components/HeroComp.vue";
    import Camps from '@/pages/campsPage.vue';
    import MapComp from '@/components/MapComp.vue';

</script>

<template>
    <div class="bg-gray-200"> <!-- a map has to be shown in this page -->
      <Hero/>
      <MapComp/>
      <!-- the "View all Camps button" will show but "the Dropdown doesn't"-->
      <Camps :limit="3" :showButton="true" :showDropdown="false"
            @setActivePage="$emit('setActivePage', $event)"
            :title="'Camps'"
      /> <!-- $event is the payload that came from the $emit() call. from campsPage to out-->
    </div> 
</template>