<script setup>
    import Hero from "@/components/HeroComp.vue";
    import Camps from '@/pages/campsPage.vue';
    import MapComp from '@/components/MapComp.vue';

</script>

<template>
    <div class="bg-gray-200"> <!-- a map has to be shown in this page -->
      <Hero/>
      <MapComp/>
      <Camps :limit="3" :showButton="true" 
            @setActivePage="$emit('setActivePage', $event)"
            :title="'Camps'"
      /> <!-- $event is the payload that came from the $emit() call. -->
    </div> 
</template>