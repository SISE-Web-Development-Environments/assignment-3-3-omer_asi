<template>
  <div class="container">
    <slot/>
    <h1 class="title w3-center w3-padding-16">Main Page</h1>
    <b-container>
      <b-row>
        <b-col>
          <RandomRecipes title="Random Recipes" class="RandomRecipes center" />
        </b-col>
        <b-col>
          <Login v-if="!$root.store.username"/>
          <LastViewedRecipes v-else title="Last Viewed Recipes"/>
    </b-col>
    </b-row>
    </b-container>
  </div>
</template>

<script>
import RandomRecipes from "../components/RandomRecipes";
import Login from "../components/Login.vue";
import LastViewedRecipes from "../components/LastViewedRecipes";
export default {
  components: {
    RandomRecipes,
    LastViewedRecipes,
    Login
  }
};
</script>

<style lang="scss" scoped>


</style>
