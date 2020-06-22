<template>
  <div id="app">
    <div id="nav" class="ml-auto">
      <router-link :to="{ name: 'main' }">Vue Recipes</router-link>|
      <router-link :to="{ name: 'search' }">Search</router-link>|
      <router-link :to="{ name: 'about' }">About</router-link>|
      <!-- {{ !$root.store.username }} -->
      <span v-if="!$root.store.username">
        Hello Guest:
        <router-link :to="{ name: 'register' }">Register</router-link>|
        <router-link :to="{ name: 'login' }">Login</router-link>|
      </span>
      <span v-else>
        {{ $root.store.username }}: 
        <b-dropdown text="Personal">
          <b-dropdown-item><router-link :to="{ name: 'favorites' }">Favorites</router-link></b-dropdown-item>
          <b-dropdown-item><router-link :to="{ name: 'myRecipes' }">My Recipes</router-link></b-dropdown-item>
          <b-dropdown-item><router-link :to="{ name: 'familyRecipes' }">Family Recipes</router-link></b-dropdown-item>
        </b-dropdown>|
        <button @click="Logout">Logout</button>|
      </span>
    </div>
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
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
