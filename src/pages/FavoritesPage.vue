<template>
  <div class="container">
    <p><br></p>
    <b-jumbotron class="title w3-center w3-padding-16"  header="Your Favorites"></b-jumbotron>
    <div v-if="recipes.length===0 && done">
    <b-jumbotron class="title w3-center w3-padding-16" header="Oops..." lead="Seems that you have not checked any recipe as favorite yet...">    </b-jumbotron>
    </div>
  <RecipePreviewList :recipes="recipes"/>
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
export default {
  
  name: "Favorites",
  components: {
    RecipePreviewList
  },
  
  data() {
    return {
      recipes: [],
      index: 0,
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
          //"http://localhost:3000/userrecipes/favorites/" 
          "https://ass32.herokuapp.com/userrecipes/favorites/"
        );
        
        //console.log(response.data);
        const recipes = response.data;
        this.recipes = [];
        var size = Object.keys(response.data).length;
        //console.log(size);
        for (let index = 1; index < size+1; index++) {
          this.recipes.push(recipes[index]);
        }
        this.done = true;
        //this.recipes.push(...recipes);
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
