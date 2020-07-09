<template>
  <b-container class="w3-main w3-content w3-padding">
    <b-col>
      <b-row v-for="r in recipes" :key="r['Id']" class='row'>
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-row>
    </b-col>
  </b-container>
</template>

<script>
import RecipePreview from "./RecipePreview.vue";
export default {
  name: "RecipePreviewList",
  components: {
    RecipePreview
  },
  props: {
    recipes: {
      type: Array,
      required: true
    }
  },
};
</script>

<style lang="scss" scoped>
b-container{
  max-width:1200px;
  margin-top:100px
}
// .container {
//   min-height: 400px;
// }
// .row {
//   min-height: 800px;
// }
</style>
