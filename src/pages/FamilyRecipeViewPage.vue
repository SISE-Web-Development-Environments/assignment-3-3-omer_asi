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
              <div>This was created by {{ recipe.owner }}</div>
              <div>We ate it on {{ recipe.occasion }}</div>
              <div v-if="recipe.time">Ready in {{ recipe.readyInMinutes }} minutes</div> 
            </div>
            Ingredients:
            <ul>
              <li>
                {{ recipe.ingredients }} 
              </li>
            </ul>
            Instructions:
            <ul>
              <li>
                {{ recipe.instructions }}
              </li>
            </ul>
            Extra informaton:
            <ul>
              <li >
                {{ recipe.extraInfo }}
              </li>
            </ul> 
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
      let image = this.$route.params.picture
      let title = this.$route.params.name
      let readyInMinutes = this.$route.params.time
      let instructions = this.$route.params.instructions
      let ingredients = this.$route.params.ingredients
      let occasion = this.$route.params.occasion
      let owner = this.$route.params.owner
      let extraInfo = this.$route.params.extraInfo

      let _recipe = {
        ingredients,
        instructions,
        readyInMinutes,
        image,
        title,
        occasion,
        owner,
        extraInfo
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
