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
          <RecipePreviewList v-else
          title="Last Viewed Recipes"
          :class="{
            RandomRecipes: true,
            blur: !$root.store.username,
            center: true
          }"
          disabled
          ></RecipePreviewList>
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
export default {
  components: {
    RandomRecipes,
    Login
  }
};
</script>

<style lang="scss" scoped>
.RandomRecipes {
  margin: 10px 0 10px;
}
.blur {
  -webkit-filter: blur(5px); /* Safari 6.0 - 9.0 */
  filter: blur(2px);
}
::v-deep .blur .recipe-preview {
  pointer-events: none;
  cursor: default;
}
</style>
