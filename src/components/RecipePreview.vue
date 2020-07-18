<template>
  <b-container class="w3-main w3-content w3-padding">
  <router-link
    :to="{ name: 'recipe', params: { recipeId: recipe.metadata.Id } }">
    <div class="recipe-body w3-bar-item" @mouseover="hover = true" @mouseleave="hover = false">
      <b-img rounded alt="Rounded image" v-if="recipe.metadata.Picture" :src="recipe.metadata.Picture" />
      <b-img rounded alt="Rounded image" v-else src='https://res.cloudinary.com/onag/image/upload/v1595063148/12753943_fp8zsx.jpg' />
    </div>
    </router-link>
    <div class="w3-row-padding w3-padding-16 w3-center">
      <h3 :title="recipe.metadata.Name" >
        {{ recipe.metadata.Name }}
      </h3>
      <p>{{ recipe.Time }} minutes | {{ recipe.Popularity }} likes<p>
      <p v-if="recipe.IsVegan">Vegan</p>
      <p v-if="recipe.IsGlutenFree">Gluten Free</p>
      <p v-else-if="!recipe.IsGlutenFree">With Gluten</p>
      <div v-if="recipe.userIndications">
        <p v-if="recipe.userIndications.DidUserWatched">Seen This</p>
        <p v-if="!recipe.userIndications.IsUserFavorite && $root.store.username"><AddToFavorites :recipe="recipe"/></p>
        <p v-if="recipe.userIndications.IsUserFavorite">Liked It</p>
      </div>
    </div>
  </b-container>
</template>

<script>
import AddToFavorites from "../components/AddToFavorites";
export default {
  components: {
    AddToFavorites
  },
  data() {
    return {
      hover: false,
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

<style>
@import 'https://www.w3schools.com/w3css/4/w3.css';
@import 'https://fonts.googleapis.com/css?family=Karma';
body,h1,h2,h3,h4,h5,h6 {font-family: "Karma", sans-serif}
.w3-bar-block .w3-bar-item {padding:20px}
p {
  font-family: "Karma", sans-serif;
  font-size: 16px;
}

img{
  width:100%
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
