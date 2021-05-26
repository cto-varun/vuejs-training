<template>
  <h1 :class="{ active: heading === true }">
    Working with Vuejs cli {{ username }}
  </h1>
  {{ typeof heading }}
  <br />
  <br />
  <button @click="changeUserName">Change Username</button>
  <br />
  <ul>
    <li v-for="book in filteredBooks">
      {{ book.name }}
    </li>
  </ul>
  <br />
  <h3>{{ message }}</h3>
  <div :class="{ displayMe: loading === true }" class="spinner"></div>
  <p>
    Email:- {{ data?.email }}
    <br />
    Gender:- {{ data?.gender }}
  </p>
  <button @click="getUsers">Fetch Users</button>
  <Contact :callUser="getUsers" />
</template>


<script>
import axios from "axios";
import Contact from "./Contact.vue";
export default {
  name: "Learning",
  props: ["user", "activeHeading"],
  components: { Contact: Contact },
  data() {
    return {
      username: this.user,
      heading: this.activeHeading,
      loading: false,
      data: {
        email: "N/A",
        gender: "N/A",
      },
      message: "",
      books: [
        { name: "C++", cost: 400 },
        { name: "ROR", cost: 500 },
        { name: "Vuejs", cost: 699 },
        { name: "Reactjs", cost: 799 },
        { name: "Angular", cost: 899 },
      ],
    };
  },
  methods: {
    changeUserName() {
      this.username = "Varun Sharma";
    },
    getUsers() {
      this.loading = true;
      this.message = "Wait while we are processing your request...";
      axios
        .get("https://randomuser.me/api")
        .then((res) => {
          // console.log("Response is ", res?.data?.results[0]?.gender);
          this.data.email = res?.data?.results[0]?.email;
          this.data.gender = res?.data?.results[0]?.gender;
          this.message = "Success!!!";
        })
        .catch((err) => {
          console.error(`Error while fetching users ${err}`, err);
          this.message = err?.message;
        })
        .finally((_) => {
          this.loading = false;
        });
    },
  },
  computed: {
    filteredBooks() {
      return this.books.filter((book) => book.cost > 600);
    },
  },
  watch: {
    username(newUsername, oldUsername) {
      // console.log(
      //   "New username ",
      //   newUsername,
      //   " and old username ",
      //   oldUsername
      // );
      if (newUsername === "Varun Sharma") {
        this.getUsers();
      }
    },
  },
};
</script>

<style scoped>
h1 {
  background: #333;
  color: #e5e5e5;
}
.active {
  background: rgb(191, 191, 191);
  color: #333;
}
.displayMe {
  display: block;
}
</style>