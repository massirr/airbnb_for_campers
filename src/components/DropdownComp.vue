<script>
    export default {
      name: "DropdownComp",
      data() {
        return {
          open: [false, false, false, false, false], // one 'open' per button hard coded 
          selected: ['', '', '', '', ''], // one 'selected' per button if I add open, I add selected
          options: ['Option 1', 'Option 2', 'Option 3', 'Option 4'],
          NamesToUseInTheDropdown: ['Select a Location', 'Select a Feature']
        }
      },
      methods: {
        toggleDropdown(dropdownindex) {
          this.open = this.open.map((val, i) => i === dropdownindex ? !val : false); // changes false to true when button is clicked in open array
        },
        selectOption(dropdownindex, option) {
          //console.log(`selected ${dropdownindex} ${option}`);
          // this.$set to ensure reactivity -> makes sure the UI updates to reflect this change after updateting the selected and open arrays.
          this.$set(this.selected, dropdownindex, option);
          this.$set(this.open, dropdownindex, false);
        },
      }
    }
</script>

<template>
  <div class="m-5">
    <div class="flex gap-4 justify-center"> <!--justify-center → center items horizontally.-->
      <div v-for="(times, dropdownindex) in 5" :key="dropdownindex" class="relative"> <!--index specifies what dropdownlist is being used-->
        <button @click="toggleDropdown(dropdownindex)"
                class="text-center w-[190px] bg-white border border-black rounded-md px-3 py-2 text-black">
          {{ selected[dropdownindex] || 'Select an option' }}
          <i v-if="!open[dropdownindex] && !selected[dropdownindex]" class="pi pi-chevron-down px-3 py-2"></i>
        </button>

        <div v-if="open[dropdownindex]" class="absolute z-10 w-[200px] bg-white border border-black mt-1 py-2 rounded-md">
          <div v-for="option in options" :key="option"
               @click="selectOption(dropdownindex, option)"
               class="px-3 py-2 hover:bg-gray-300 cursor-pointer rounded-md">
            {{ option }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>