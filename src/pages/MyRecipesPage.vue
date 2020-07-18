<template>
  <div class="container">
    <h1 class="title">My Recipes</h1>
  <b-container>
    <b-col >
      <b-row v-for="r in recipes" :key="r.name">
        <UserRecipePreview class="recipePreview" :recipe="r" />
      </b-row>
    </b-col>
  </b-container>
  </div>
</template>

<script>
import UserRecipePreview from "../components/UserRecipePreview";
export default {
  
  name: "Favorites",
  components: {
    UserRecipePreview
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
          "http://localhost:3000/userrecipes/userPrivateRecipes/" 
          //"https://ass32.herokuapp.com/userrecipes/userPrivateRecipes/"
        );
        
        const recipes = response.data;
        this.recipes = [];
        var size = Object.keys(response.data).length;
        for (let index = 1; index < size+1; index++) {
          this.recipes.push(recipes[index]);
        }
        //this.recipes.push(...recipes);
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
