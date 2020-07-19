<template>
  <div class="container">
    <p><br></p>
    <b-jumbotron class="title w3-center w3-padding-16"  header="Family Recipes"></b-jumbotron>
    <div v-if="recipes.length===0 && done">
    <b-jumbotron class="title w3-center w3-padding-16" header="Oops..." lead="Seems that you have not entered any family recipes yet...">    </b-jumbotron>
    </div>
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
      recipes: [],
      done: false

    };
  },
  mounted() {
    this.updateRecipes();
  },
  methods: {
    async updateRecipes() {
      try {
        const response = await this.axios.get(
         //"http://localhost:3000/userrecipes/familyRecipes/" 
          "https://ass32.herokuapp.com/userrecipes/familyRecipes/"
        );
        
        //console.log(response);
        const recipes = response.data;
        this.recipes = [];
        var size = Object.keys(response.data).length;
        //console.log(size);
        for (let index = 1; index < size+1; index++) {
          this.recipes.push(recipes[index]);
        }
        this.done = true;
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
