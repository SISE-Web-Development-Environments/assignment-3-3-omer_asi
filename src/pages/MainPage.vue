<template>
  <div class="container">
    <slot/>
    <p></p>
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
