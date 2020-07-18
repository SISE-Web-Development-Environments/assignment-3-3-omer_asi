<template>
  <div class="container">
    <p><br></p>
    <b-jumbotron class="title w3-center w3-padding-16"  header="My Recipes"></b-jumbotron>
    <div v-if="recipes.length===0 && done">
    <b-jumbotron class="title w3-center w3-padding-16" header="Opss..." lead="Seems that you have not entered any private recipes yet...">    </b-jumbotron>
    </div>
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
          "http://localhost:3000/userrecipes/userPrivateRecipes/" 
          //"https://ass32.herokuapp.com/userrecipes/userPrivateRecipes/"
        );
        
        const recipes = response.data;
        this.recipes = [];
        var size = Object.keys(response.data).length;
        for (let index = 1; index < size+1; index++) {
          this.recipes.push(recipes[index]);
        }
        this.done = true;
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
