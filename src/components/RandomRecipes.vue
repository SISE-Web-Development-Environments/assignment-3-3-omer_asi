<template>
  <b-container>
    <b-jumbotron class="title w3-center w3-padding-16"  header="Explore these recipes"></b-jumbotron>
    <RecipePreviewList :recipes="recipes"/>
    <b-button id="random" @click="updateRecipes">Change Recipes</b-button>
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
          //"http://localhost:3000/recipes/RandomRecipes/"
           "https://ass32.herokuapp.com/recipes/RandomRecipes/"
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

#random {
  background: green;
  height: 70%;
  font-size: 25px;
  margin: auto;
  display: flex;
}
</style>
