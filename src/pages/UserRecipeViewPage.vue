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
              <div>Number Of Dishes: {{ recipe.numOfDished }} dishes</div>
              <div v-if="recipe.vegan">This meal is Vegan :)</div>
              <div v-if="recipe.glutenFree">This meal Gluten Free :)</div>
              
            </div>
            Ingredients:
            <ul>
              <li v-for="line in splitedIng" :key="line">
                {{ line }} 
              </li>
            </ul>
            Instructions:
            <ol>
              <li v-for="line in splitedIns" :key="line">
                {{ line }} 
              </li>
            </ol>
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
  computed: {
    // a computed getter
    splitedIng: function () {
      return this.$route.params.ingredients.split('\\n');
    },
    splitedIns: function () {
      return this.$route.params.instructions.split('\\n');
    }
  },
  async created() {
    try {
      let image = this.$route.params.picture
      let title = this.$route.params.name
      let readyInMinutes = this.$route.params.time
      let instructions = this.$route.params.instructions
      let ingredients = this.$route.params.ingredients
      let vegan = this.$route.params.IsVegan
      let glutenFree = this.$route.params.IsGlutenFree
      let numOfDished = this.$route.params.numdishes
      let extraInfo = this.$route.params.extraInfo

      let _recipe = {
        ingredients,
        instructions,
        readyInMinutes,
        image,
        title,
        vegan,
        glutenFree,
        numOfDished,
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
