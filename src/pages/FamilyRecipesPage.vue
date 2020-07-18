<template>
  <div class="container">
    <h1 class="title">Family Recipes</h1>
  <b-container>
    <b-col >
      <b-row v-for="r in recipes" :key="r.name">
        <FamilyRecipePreview class="recipePreview" :recipe="r" />
      </b-row>
    </b-col>
  </b-container>
  </div>
</template>

<script>
import FamilyRecipePreview from "../components/FamilyRecipePreview";
export default {
  
  name: "Favorites",
  components: {
    FamilyRecipePreview
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
          "http://localhost:3000/userrecipes/familyRecipes/" 
          //"https://ass32.herokuapp.com/userrecipes/familyRecipes/"
        );
        
        //console.log(response);
        const recipes = response.data;
        this.recipes = [];
        var size = Object.keys(response.data).length;
        //console.log(size);
        for (let index = 1; index < size+1; index++) {
          this.recipes.push(recipes[index]);
        }
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
</style>
