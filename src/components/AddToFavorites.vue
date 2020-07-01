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
                console.log(this.recipe)
                const response = await this.axios.post(
                  "http://localhost:3000/userrecipes/addfavorites/" + this.recipe.metadata.Id, { withCredentials: true }
                  //"https://ass32.herokuapp.com/recipes/RandomRecipes/"
                );
                console.log(response);
                //this.$router.go()
                } catch (error) {
                  console.log(error);
                }
            }
        },
        props: {
            recipe: {
                type: Object,
                required: true
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>