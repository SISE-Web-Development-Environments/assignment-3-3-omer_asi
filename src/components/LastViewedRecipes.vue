<template>
  <b-container>
    <b-jumbotron class="title w3-center w3-padding-16"  header="Last Viewed Recipes"></b-jumbotron>
    <div v-if="recipes.length===0 && done">
    <b-jumbotron class="title w3-center w3-padding-16" header="Hey newbie..." lead="Your last watched recipes will appear here">    </b-jumbotron>
    </div>
    <RecipePreviewList :recipes="recipes"/>
  </b-container>
</template>

<script>
import RecipePreviewList from "./RecipePreviewList.vue";
export default {
  name: "LastViewedRecipes",
  components: {
    RecipePreviewList
  },
  props: {
    title: {
      type: String,
      required: true
    }
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
          //"http://localhost:3000/userrecipes/lastRecipes"
           "https://ass32.herokuapp.com/userrecipes/lastRecipes"
        );

        const recipes = response.data;
        this.recipes = [];
        var size = Object.keys(response.data).length;
        //console.log(size);
        for (let index = 1; index < size+1; index++) {
          this.recipes.push(recipes[`Recipe ${index}`]);
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
.row {
  min-height: 400px;
}
</style>
