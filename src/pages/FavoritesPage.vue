<template>
  <div class="container">
    <h1 class="title">Favorites Page</h1>
  <!-- <b-container v-if="(recipes.length)%5==0">
    <b-row v-for="i in (recipes.length)/5" :key="i">
      <b-col v-for="r in i" :key="recipes[r]">
        <RecipePreview class="recipePreview" :recipe="recipes[r]" />
      </b-col>
    </b-row>
  </b-container>
  <b-container else>
    <b-row v-for="i in (recipes.length)/5+1" :key="i">
      <b-col v-for="r in i" :key="recipes[r]">
        <RecipePreview class="recipePreview" :recipe="recipes[r]" />
      </b-col>
    </b-row>
  </b-container> -->
  <RecipePreviewList :recipes="recipes"/>
  </div>
</template>

<script>
import RecipePreview from "../components/RecipePreview";
import RecipePreviewList from "../components/RecipePreviewList";
export default {
  
  name: "Favorites",
  components: {
    //RecipePreview,
    RecipePreviewList
  },
  
  data() {
    return {
      recipes: [],
      index: 0
    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        const response = await this.axios.get(
          "http://localhost:3000/userrecipes/favorites/" 
          //"https://ass32.herokuapp.com/userrecipes/favorites/"
        );
        
        console.log(response.data);
        const recipes = response.data;
        this.recipes = [];
        var size = Object.keys(response.data).length;
        console.log(size);
        for (let index = 1; index < size+1; index++) {
          this.recipes.push(recipes[index]);
        }
        //this.recipes.push(...recipes);
        console.log(this.recipes);
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
</style>
