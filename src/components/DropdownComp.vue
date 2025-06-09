<script>
  export default {
    name: "DropdownComp",
    props: {
      emitFeatureID: {
        type: Boolean,
        default: false, // Default to emitting featureName
      },
    },
    data() {
      return {
        checkboxes: [], // Array to keep track of which checkboxes are checked
        features: [],   // Array to store the features fetched from the server
        open: false     // Controls dropdown open/close
      }
    },
    mounted() {
      this.fetchFeatures();
    },
    computed: {
      uniqueFeatures() {
        // Only keep the first feature with each name (remove duplicates)
        const seen = new Set();
        return this.features.filter(f => {
          const name = f.features.featureName;
          if (seen.has(name)) return false;
          seen.add(name);
          return true;
        });
      }
    },
    methods: {
      fetchFeatures() {
        fetch("http://localhost:3000/features", {
          method: "GET"
        })
          .then(response => response.json())
          .then(data => {
            this.features = data;
            this.checkboxes = Array(data.length).fill(false);
          })
          .catch(error => {
            console.error("Error fetching features:", error);
          });
      },
      toggleDropdown() {
        this.open = !this.open;
      }
    }
  }
</script>

<template>
  <div class="m-5 w-full max-w-xl">
    <!-- Dropdown Button -->
    <button @click="toggleDropdown" class="flex items-center justify-between w-full bg-white border border-gray-400 rounded-md px-4 py-2 text-black shadow">
      <span>Select Features</span>
      <i :class="['pi', open ? 'pi-chevron-up' : 'pi-chevron-down', 'ml-2']"></i> <!-- this handles the arrow near the dropdown to move up and down-->
    </button>
    <!-- Dropdown Content -->
    <div v-if="open" class="mt-2 border border-gray-300 rounded-md bg-white shadow-lg p-4">
      <div class="grid grid-cols-2 md:grid-cols-3 gap-4">
        <!--When the checkbox is checked or unchecked, the value at checkboxes[idx] is updated to true or false, respectively.-->
        <div v-for="(feature, idx) in uniqueFeatures" :key="feature.features.featureName" class="flex items-center gap-2">
          <input
            type="checkbox"
            v-model="checkboxes[idx]"
            :id="'cb-' + idx"
            class="accent-black"
            @change="$emit('feature-selected', emitFeatureID ? feature.features.featureID : feature.features.featureName, checkboxes[idx])"
          />
          <!--When the checkbox's state changes, the change event is triggered.-->
          <label :for="'cb-' + idx">{{ feature.features.featureName }}</label>
        </div>
      </div>
    </div>
  </div>
</template>

<!--size of the icon-->
<style scoped>
.pi { 
  font-size: 1.2em;
}
</style>