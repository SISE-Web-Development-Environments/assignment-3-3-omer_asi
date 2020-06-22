<template>
  <div class="container">
    <div v-if="recipe">
      <div class="recipe-header mt-3 mb-4">
        <h1>{{ recipe.title }}</h1>
        <img :src="recipe.image" class="center" />
      </div>
      <div class="recipe-body">
        <div class="wrapper">
          <div class="wrapped">
            <div class="mb-3">
              <div>Ready in {{ recipe.readyInMinutes }} minutes</div>
              <div>Number Of Dishes: {{ recipe.numOfDished }} dishes</div>
              <div v-if="recipe.vegan">This meal is Vegan :)</div>
              <div v-if="recipe.glutenFree">This meal Gluten Free :)</div>
              
            </div>
            Ingredients:
            <ul>
              <li>
                {{ recipe.ingredients }} 
              </li>
            </ul>
            Instructions:
            <ol>
              <li >
                {{ recipe.instructions }}
              </li>
            </ol>
            Extra informaton:
            <ol>
              <li >
                {{ recipe.extrainfo }}
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
export default {
  data() {
    return {
      recipe: null
    };
  },
  async created() {
    try {
      // let response;
      // response = this.$route.params.response;
      // try {
      //   //console.log(this.$route.params.recipeId)
      //   response = await this.axios.get(
      //     "http://localhost:3000/recipes/" + this.$route.params.recipeId, { withCredentials: true }
      //     //"https://ass32.herokuapp.com/recipes/" + this.$route.params.recipeId, { withCredentials: true }
      //   );

      //   // console.log("response.status", response.status);
      //   if (response.status !== 200) this.$router.replace("/NotFound");
      // } catch (error) {
      //   console.log("error.response.status", error.response.status);
      //   this.$router.replace("/NotFound");
      //   return;
      // }
      
      let image = response.data.FullRecipe.RecipePreview.metadata.Picture
      let title = response.data.FullRecipe.RecipePreview.metadata.Name
      let readyInMinutes = response.data.FullRecipe.RecipePreview.Time
      let instructions = response.data.FullRecipe.RecipeInner.instuctions
      let ingredients = response.data.FullRecipe.RecipeInner.ingredients
      let vegan = response.data.FullRecipe.RecipePreview.IsVegan
      let glutenFree = response.data.FullRecipe.RecipePreview.IsGlutenFree
      let numOfDished = response.data.FullRecipe.RecipeInner.numdishes
      let extrainfo = response.data.FullRecipe.RecipeInner.numdishes

      let _recipe = {
        ingredients,
        instructions,
        readyInMinutes,
        image,
        title,
        vegan,
        glutenFree,
        numOfDished,
        extrainfo
      };

      this.recipe = _recipe;
    } catch (error) {
      console.log(error);
    }
  }
};
</script>

<style scoped>
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
