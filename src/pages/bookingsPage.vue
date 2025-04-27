<script>
    export default {
      name: 'bookingsPage',
      props: {
        bookedItems: null  
      },
      mounted() {
        if(this.bookedItems) {
          this.bookedItems.forEach((item) => { // each item is emited at mount
            this.changeState(item)
          })
        }
      },
      methods: {
        changeState(item) {
          this.$emit('bookedState', item)
          //console.log('Emitting bookedItem:', item)
        }
      },
    }
</script>

<template>
  <div>
    <div class="min-h-screen bg-blue-100 flex items-center justify-center p-4">
      <div class="max-w-md w-full bg-gray-200 rounded-xl shadow-lg p-8">
        <h2 
          v-for="(item, index) in bookedItems" :key="index"
          class="text-2xl font-bold text-gray-900 mb-6 text-center"
        >
        {{ item }}
        </h2>
      </div>
    </div>
  </div>
</template>