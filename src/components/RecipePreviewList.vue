<template>
  <b-container v-if="size<=3">
    <b-col>
      <b-row v-for="r in recipes" :key="r['Id']" class='row'>
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-row>
    </b-col>
  </b-container>
  <b-container v-else>
    <b-row>
      <b-col v-for="i in size" :key="recipes[i]" class='row'>
        <RecipePreview class="recipePreview" :recipe="recipes[i]" />
      </b-col>
    </b-row>
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
 data() {
    return {
      size: 0,
    };
  },
  mounted() {
    this.size = this.recipes.length;
    console.log("yoyo"+this.size)
    //console.log(this.recipes[0].metadata.Id)
  }
};
</script>

<style lang="scss" scoped>

.container {
  min-height: 400px;
}
.row {
  min-height: 400px;
}
</style>
