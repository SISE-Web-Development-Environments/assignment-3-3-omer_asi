<template>
    <div>
            <button @click="addToFavorites">Like This?</button>  
    </div>
</template>

<script>
    export default {
        methods: {
            async addToFavorites() {
                try {
                console.log(this.$recipe)
                const response = await this.axios.get(
                  "http://localhost:3000/userrecipes/addFavorites/" + this.$recipe.metadata.recipeId, { withCredentials: true }
                  //"https://ass32.herokuapp.com/recipes/RandomRecipes/"
                );
                console.log(response);
                
                console.log(this.recipes);
                } catch (error) {
                  console.log(error);
                }
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>