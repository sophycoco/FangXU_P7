<template>
  <div class="navigation">
    <nav class="navbar">
      <div class="navbar-collapse">
        <ul>
          <li class="nav-item">
            <router-link class="nav-link" :to="`/user/${usersid}`">Profile</router-link>
          </li>
          <li class="nav-item" @click="Logout()">
            <!--<button class="nav-link" @click="Logout()">-->Logout
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/Home">Home</router-link>
          </li>
        </ul>
      </div>
    </nav>
  </div>
</template>
<script>
import VueJwtDecode from "vue-jwt-decode";

export default {
  name: "Nav",
  data() {
    return {
      usersid: VueJwtDecode.decode(localStorage.getItem("token")).userId,
    };
  },
  methods: {
    Logout() {
      localStorage.clear();
      this.$router.push("/");
    },
  },
};
</script>

<style scoped>
.navigation {
  margin:30px 0 100px 0;
}
.navbar-collapse {
  width:40%;
  margin:auto;
}
ul {
  padding: 0;
  display: flex;
  justify-content:space-evenly;
}
.nav-item
 {
  text-decoration: none;
  list-style: none;
  color: #2c3e50;
}
.nav-item:hover, .nav-link:hover {
  background-color: #ff5050e5;
  color:whitesmoke;
  font-style: bold;
  padding: 2px 5px;
  border-radius: 4px;  
}
.nav-link
 {
  text-decoration: none;
  list-style: none;
  color: #2c3e50;
}
@media only screen and (max-width: 1024px) {
  .navigation {
  margin:15px 0 35px 0;
}
  ul {
  padding: 0;
  display: flex;
  font-size: 13px;
  justify-content:space-between;
}
}

</style>
