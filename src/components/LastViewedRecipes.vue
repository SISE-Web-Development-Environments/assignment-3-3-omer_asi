<template>
  <b-container>
    <!-- <h3>Hi {{ $root.store.username }}</h3> -->
    <b-jumbotron class="title w3-center w3-padding-16"  header="Last Viewed Recipes"></b-jumbotron>
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
          "http://localhost:3000/userrecipes/lastRecipes"
           //"https://ass32.herokuapp.com/userrecipes/lastRecipes"
        );

        const recipes = response.data;
        this.recipes = [];
        var size = Object.keys(response.data).length;
        //console.log(size);
        for (let index = 1; index < size+1; index++) {
          this.recipes.push(recipes[`Recipe ${index}`]);
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
.row {
  min-height: 400px;
}
</style>
