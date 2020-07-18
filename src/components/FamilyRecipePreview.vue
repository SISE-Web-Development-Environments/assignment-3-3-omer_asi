<template>
  <b-container class="w3-main w3-content w3-padding">
  <router-link
    :to="{ name: 'familyUserRecipe', params: { name: recipe.name, picture: recipe.picture, time: recipe.time, instructions: recipe.instructions,
                                  ingredients: recipe.ingredients, extraInfo: recipe.extraInfo, occasion: recipe.occasion, owner: recipe.owner } }">
    <div class="recipe-body w3-bar-item" @mouseover="hover = true" @mouseleave="hover = false">
      <b-img rounded alt="Rounded image" :src="recipe.picture" class="recipe-image" />
    </div>
  </router-link>
    <div class="w3-row-padding w3-padding-16 w3-center">
      <h3 :title="recipe.name" class="recipe-title">
        {{ recipe.name }}
      </h3>
      <p v-if="recipe.time">{{ recipe.time }} minutes</p>
    </div>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
    };
  },
  props: {
    recipe: {
      type: Object,
      required: true
    }

  }
};
</script>

<style scoped>
@import 'https://www.w3schools.com/w3css/4/w3.css';
@import 'https://fonts.googleapis.com/css?family=Karma';
body,h1,h2,h3,h4,h5,h6 {font-family: "Karma", sans-serif}
.w3-bar-block .w3-bar-item {padding:20px}
p {
  font-family: "Karma", sans-serif;
  font-size: 20px;
  font-weight: bold;
}

img {
  max-height: 286.81px;
  max-width: 431px;
  display: block;
  margin-left: auto;
  margin-right: auto;
}

.recipe-body:hover {
  background: white;
  opacity: 0.3;
  display: block;
  width: 100%;
  transition: .5s ease;
  backface-visibility: hidden;
  
}
</style>
