<template>
  <div class="container">
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <b-jumbotron class="title w3-center w3-padding-16"  :header="recipe.title"></b-jumbotron>
        <b-img rounded alt="Rounded image" :src="recipe.image" class="center" />
      </div>
      <div class="">
        <div class="wrapper">
          <div class="wrapped">
            <div class="mb-3">
              <div>Ready in {{ recipe.readyInMinutes }} minutes</div>
              <div>Likes: {{ recipe.aggregateLikes }} likes</div>
              <div >Number Of Dishes: {{ recipe.numOfDished }} dishes</div>
              <div v-if="recipe.vegan">This meal is Vegan :)</div>
              <div v-if="recipe.glutenFree">This meal Gluten Free :)</div>
              <div v-if="recipe.recipeViewed">This Recipe was already viewed</div>
              <div id="favs">
                <div v-if="favorite" >This is one of your favorites</div>
                <div v-if="!favorite && $root.store.username"><AddToFavorites class="RandomRecipes center" :recipe="recipe"/></div>
              </div>
            </div>
            Ingredients:
            <ul>
              <li
                v-for="(r, index) in recipe.extendedIngredients"
                :key="index + '_' + r.id"
              >
                {{ r.original }}
              </li>
            </ul>
          </div>
          <div class="wrapped">
            Instructions:
            <ol>
              <li v-for="s in recipe._instructions" :key="s.number">
                {{ s.step }}
              </li>
            </ol>
          </div>
        </div>
      </div>
      <!-- <pre>
      {{ $route.params }}
      {{ recipe }}
    </pre
      > -->
    </div>
  </div>
</template>

<script>
import AddToFavorites from "../components/AddToFavorites";
export default {
  components: {
    AddToFavorites
  },
  data() {
    return {
      recipe: null,
      favorite: false
    };
  },
  async created() {
    try {
      let response;
      // response = this.$route.params.response;

      try {
        //console.log(this.$route.params.recipeId)
        response = await this.axios.get(
          "http://localhost:3000/recipes/" + this.$route.params.recipeId, { withCredentials: true }
          //"https://ass32.herokuapp.com/recipes/" + this.$route.params.recipeId, { withCredentials: true }
        );

        // console.log("response.status", response.status);
        if (response.status !== 200) this.$router.replace("/NotFound");
      } catch (error) {
        console.log("error.response.status", error.response.status);
        this.$router.replace("/NotFound");
        return;
      }
      //console.log(document.cookie)
      //console.log(Vue.$cookies.get("session"))
      //console.log(response)
      //console.log(this.recipe)
      let metadata = response.data.FullRecipe.RecipePreview.metadata
      let image = response.data.FullRecipe.RecipePreview.metadata.Picture
      let title = response.data.FullRecipe.RecipePreview.metadata.Name
      let readyInMinutes = response.data.FullRecipe.RecipePreview.Time
      let aggregateLikes = response.data.FullRecipe.RecipePreview.Popularity
      let analyzedInstructions = response.data.FullRecipe.RecipeInner.analyzedInstructions
      let instructions = response.data.FullRecipe.RecipeInner.instuctions
      let extendedIngredients = response.data.FullRecipe.RecipeInner.ingredients
      let vegan = response.data.FullRecipe.RecipePreview.IsVegan
      let glutenFree = response.data.FullRecipe.RecipePreview.IsGlutenFree
      let numOfDished = response.data.FullRecipe.RecipeInner.numdishes

      let recipeViewed
      let isFavorites 
      if (response.data.FullRecipe.RecipePreview.userIndications){
        recipeViewed = response.data.FullRecipe.RecipePreview.userIndications.DidUserWatched
        isFavorites = response.data.FullRecipe.RecipePreview.userIndications.IsUserFavorite
        this.favorite = isFavorites
      }

      let _instructions = analyzedInstructions
        .map((fstep) => {
          fstep.steps[0].step = fstep.name + fstep.steps[0].step;
          return fstep.steps;
        })
        .reduce((a, b) => [...a, ...b], []);

      let _recipe = {
        instructions,
        _instructions,
        analyzedInstructions,
        extendedIngredients,
        aggregateLikes,
        readyInMinutes,
        image,
        title,
        vegan,
        glutenFree,
        numOfDished,
        recipeViewed,
        isFavorites,
        metadata
      };

      this.recipe = _recipe;
    } catch (error) {
      console.log(error);
    }
  }
};
</script>

<style scoped>
div{
  font-family: "Karma", sans-serif;
  font-size: 20px;
  font-weight: bold;
}

.wrapper {
  display: flex;
}
.wrapped {
  width: 50%;
}
.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
}
/* .recipe-header{

} */
</style>
