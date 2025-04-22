<script setup>
    import Hero from "@/components/HeroComp.vue";
    import CampComp from '@/components/CampComp.vue';
    import MapComp from '@/components/MapComp.vue';

</script>

<template>
    <div class="bg-gray-100"> <!-- a map has to be shown in this page -->
      <Hero/>
      <MapComp/>
      <CampComp/>
    </div>
</template>