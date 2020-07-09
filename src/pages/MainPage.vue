<template>
  <div class="container">
    <h1 class="title">Main Page</h1>
    <b-container>
      <b-row>
        <b-col>
          <RandomRecipes title="Random Recipes" class="RandomRecipes center" />
        </b-col>
        <b-col>
          <Login v-if="!$root.store.username"/>
          <!-- {{ !$root.store.username }} -->
          <LastViewedRecipes v-else title="Last Viewed Recipes"/>
    <!-- <div
      style="position: absolute;top: 70%;left: 50%;transform: translate(-50%, -50%);"
    >
      Centeredasdasdad
    </div>-->
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
