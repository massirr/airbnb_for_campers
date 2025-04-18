<script> // replaces export default -> after importing, there is always an export default for it to be used
    import logo from '@/assets/image/tent.png'; // @ alias for src folder

    export default {
      name: "NavigationComponent",
      data() {
        return {
          items: [
            "home",
            "account"
          ],
          logo: logo
        }
      },
      methods: {
        setActivePage(page) {
          this.$emit("setActivePage", page);
        }
      }
    }
</script>

<template>
    <nav class="bg-green-700 border-b border-green-500">
      <div class="mx-auto max-w-7xl px-2 sm:px-6 lg:px-8">
        <div class="flex h-20 items-center justify-between">
          <div
            class="flex flex-1 items-center justify-center md:items-stretch md:justify-start">

            <!-- Logo -->
            <a class="flex flex-shrink-0 items-center mr-4">
              <img class="h-10 w-auto" :src="logo"/>
              <span class="hidden md:block text-white text-2xl font-bold ml-2"
                >AirBnb For Campers</span>
            </a>
            <div class="md:ml-auto">
              <div class="flex space-x-2">
                <button                  
                  v-for="(item, index) in items"
                  :key="index"
                  @click="setActivePage(item)"
                  class="text-white bg-green-900 hover:bg-gray-900 hover:text-white rounded-md px-3 py-2"
                  >
                  {{item}}
                </button>
                <input 
                    type="text"
                    class="text-black hover:bg-green-900 hover:text-white rounded-md px-3 py-2" 
                    placeholder="Search...">
              </div>

            </div>
            <a class="absolute right-10 text-white hover:bg-green-900 hover:text-white rounded-md px-3 py-2"> account </a>
          </div>
        </div>
      </div>
    </nav>
</template>