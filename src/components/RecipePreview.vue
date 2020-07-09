<template>
  <router-link
    :to="{ name: 'recipe', params: { recipeId: recipe.metadata.Id } }"
    class="recipe-preview"
   >
    <div class="recipe-body" @mouseover="hover = true" @mouseleave="hover = false">
      <img :src="recipe.metadata.Picture" class="recipe-image image" />
    </div>
    <div class="recipe-footer">
      <div :title="recipe.metadata.Name" class="recipe-title">
        {{ recipe.metadata.Name }}
      </div>
      <ul class="recipe-overview">
        <li>{{ recipe.Time }} minutes</li>
        <li>{{ recipe.Popularity }} likes</li>
      </ul>
      <ul class="recipe-overview">
        <li v-if="recipe.IsVegan">Vegan</li>
        <li v-if="recipe.IsGlutenFree">Gluten Free</li>
        <li v-else-if="!recipe.IsGlutenFree">With Gluten</li>
      </ul>
      <ul v-if="recipe.userIndications" class="recipe-overview">
        <li v-if="recipe.userIndications.DidUserWatched">Seen This</li>
        <li v-if="!recipe.userIndications.IsUserFavorite"><AddToFavorites class="RandomRecipes center" :recipe="recipe"/></li>
        <li v-if="recipe.userIndications.IsUserFavorite">Liked It</li>
      </ul>
    </div>
  </router-link>
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

<style scoped>
.recipe-preview {
  display: inline-block;
  width: 90%;
  height: 100%;
  position: relative;
  margin: 10px 10px;
}
.recipe-preview > .recipe-body {
  width: 100%;
  height: 200px;
  position: relative;
}

.recipe-preview .recipe-body .recipe-image {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
  display: block;
  width: 98%;
  height: auto;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  background-size: cover;
}

.recipe-preview .recipe-footer {
  width: 100%;
  height: 50%;
  overflow: hidden;
}

.recipe-preview .recipe-footer .recipe-title {
  padding: 10px 10px;
  width: 100%;
  font-size: 12pt;
  text-align: left;
  white-space: nowrap;
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
}

.recipe-preview .recipe-footer ul.recipe-overview {
  padding: 5px 10px;
  width: 100%;
  display: -webkit-box;
  display: -moz-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  box-flex: 1;
  -webkit-flex: 1 auto;
  -ms-flex: 1 auto;
  flex: 1 auto;
  table-layout: fixed;
  margin-bottom: 0px;
}

.recipe-preview .recipe-footer ul.recipe-overview li {
  -webkit-box-flex: 1;
  -moz-box-flex: 1;
  -o-box-flex: 1;
  -ms-box-flex: 1;
  box-flex: 1;
  -webkit-flex-grow: 1;
  flex-grow: 1;
  width: 90px;
  display: table-cell;
  text-align: center;
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
