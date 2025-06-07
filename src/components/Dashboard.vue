<script>

  export default {
    name: "DashboardComp",
    data() {
      return {
        users: null,
      }
    },
    mounted() {
      this.getUsers()
    },
    methods: {
      getUsers() {
        fetch("http://localhost:3000/users/", {
          method: "GET"
        })
            .then((response) => response.json())
            .then((_users) => {
              this.users = _users
              console.log(this.users)
            })
            .catch((error) => {
              console.error("Error fetching camps:", error); // Handle errors
            });
      },
      truncatedText(text) {
        const maxLength = 10; // Set the maximum number of characters
        return text.length > maxLength
          ? text.substring(0, maxLength) + "..." //substring uses indices
          : text;
      },
    }
  };
</script>

<template>
  <div>
    <h1 class="text-2xl font-bold text-gray-800 mb-6 text-center">Dashboard</h1>
    <table class="table-auto border-collapse border border-gray-300 w-full text-center">
      <thead>
        <tr>
          <th class="border border-gray-300 px-4 py-2">userID</th>
          <th class="border border-gray-300 px-4 py-2">username</th>
          <th class="border border-gray-300 px-4 py-2">email</th>
          <th class="border border-gray-300 px-4 py-2">password</th>
          <th class="border border-gray-300 px-4 py-2">createDate</th>
          <th class="border border-gray-300 px-4 py-2">isAdmin</th>

        </tr>
      </thead>
      <tbody>
        <tr v-for="(user, index) in users" :key="index">
          <td class="border border-gray-300 px-4 py-2">{{ user.userID }}</td>
          <td class="border border-gray-300 px-4 py-2">{{ user.username }}</td>
          <td class="border border-gray-300 px-4 py-2">{{ user.email }}</td>
          <td class="border border-gray-300 px-4 py-2">{{ truncatedText(user.password) }}</td>
          <td class="border border-gray-300 px-4 py-2">{{ user.createDate }}</td>
          <td class="border border-gray-300 px-4 py-2">{{ user.isAdmin }}</td>
        </tr>
      </tbody>
   </table>
  </div>
</template>
