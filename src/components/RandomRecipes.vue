<template>
  <b-container>
    <h3>
      {{ title }}:
      <slot></slot>
    </h3>
    <RecipePreviewList :recipes="recipes"/>
  </b-container>
</template>

<script>
import RecipePreviewList from "./RecipePreviewList.vue";
export default {
  name: "RandomRecipes",
  components: {
    RecipePreviewList
  },
  props: {
    title: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      recipes: []
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        const response = await this.axios.get(
          "http://localhost:3000/recipes/RandomRecipes/"
           //"https://ass32.herokuapp.com/recipes/RandomRecipes/"
        );

        //console.log(response.data);
        const recipes = response.data;
        this.recipes = [];
        this.recipes.push(recipes['Random Recipe 1']);
        this.recipes.push(recipes['Random Recipe 2']);
        this.recipes.push(recipes['Random Recipe 3']);
        //console.log(this.recipes);
      } catch (error) {
        console.log(error);
      }
    }
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
