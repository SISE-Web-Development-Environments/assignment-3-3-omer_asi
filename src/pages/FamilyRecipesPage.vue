<template>
  <div class="container">
    <h1 class="title">Family Recipes Page</h1>
    
  <b-container>
    <b-row >
      <b-col v-for="r in recipes" :key="r.metadata.Id">
        <router-link
          :to="{ name: 'recipe', params: { recipeId: recipe.metadata.Id } }"
          class="recipe-preview"
        >
          <div class="recipe-body">
            <img :src="recipe.metadata.Picture" class="recipe-image" />
          </div>
          <div class="recipe-footer">
            <div :title="recipe.metadata.Name" class="recipe-title">
              {{ recipe.metadata.Name }}
            </div>
            <ul class="recipe-overview">
              <li>{{ recipe.Time }} minutes</li>
              <li>{{ recipe.Popularity }} likes</li>
            </ul>
          </div>
        </router-link>
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
  </b-container>
  </div>
</template>

<script>
import RecipePreview from "../components/RecipePreview";
export default {
  
  name: "Favorites",
  components: {
    RecipePreview
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
