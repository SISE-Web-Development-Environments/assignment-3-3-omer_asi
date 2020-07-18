<template>
  <div id="app" v-bind:style="{ backgroundImage: 'url(https://res.cloudinary.com/onag/image/upload/v1595090657/lukas-blazek-f-TWhXOrLiU-unsplash_dcng1p.jpg)' }">
    <b-navbar toggleable="lg" type="dark" variant="success">

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
      <b-collapse id="nav-collapse" is-nav>

      <b-navbar-nav>
        <b-nav-item :to="{ name: 'main' }">Home</b-nav-item>
        <b-nav-item :to="{ name: 'search' }">Search</b-nav-item>
        <b-nav-item :to="{ name: 'about' }">About</b-nav-item>
      </b-navbar-nav>

      <b-navbar-nav class="ml-auto" v-if="!$root.store.username">
        <b-navbar-brand>Hello Guest</b-navbar-brand>
        <b-nav-item :to="{ name: 'register' }">Register</b-nav-item>
        <b-nav-item :to="{ name: 'login' }">Login</b-nav-item>
      </b-navbar-nav>
      
      <b-navbar-nav class="ml-auto" v-else>
        <b-navbar-brand>Hello {{ $root.store.username }}</b-navbar-brand> 
        <b-nav-item-dropdown text="Personal" right>
          <b-dropdown-item :to="{ name: 'favorites' }">Favorites</b-dropdown-item>
          <b-dropdown-item :to="{ name: 'myRecipes' }">My Recipes</b-dropdown-item>
          <b-dropdown-item :to="{ name: 'familyRecipes' }">Family Recipes</b-dropdown-item>
        </b-nav-item-dropdown>
        <b-nav-item @click="Logout">Logout</b-nav-item>
      </b-navbar-nav>

      </b-collapse>
    </b-navbar>
    <router-view />
  </div>
</template>

<script>
export default {
  name: "App",
  methods: {
    async Logout() {
      const response = await this.axios.post(
          //"https://ass32.herokuapp.com/auth/Logout",
          "http://localhost:3000/auth/Logout",
          {
            withCredentials: true
          }
      );
      console.log(response)
      console.log(document.cookie)
      this.$root.store.logout();
      this.$root.toast("Logout", "User logged out successfully", "success");
      this.$router.push("/").catch(() => {
        this.$forceUpdate();
      });
    }
  }
};
</script>

<style lang="scss">
@import "@/scss/form-style.scss";

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
  background-repeat: no-repeat;
  background-size:2300px 1100px;
  background-position: center;
  background-attachment: fixed;
}
</style>
