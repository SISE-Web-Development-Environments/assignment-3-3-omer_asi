<template>
    <div> 
        <button slot="icon" @click="addToFavorites" >Like This?</button>
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
                if(this.recipe.userIndications){
                    this.recipe.userIndications.IsUserFavorite=true;
                }
                else{
                     this.recipe.isFavorites=true;
                }
                //console.log(response);
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
button{
  border: 1px solid #ddd;
  color: #333;
  background-color:#fff;
  border-radius: 2px;
  font-size: 14px;
  font-family: '微软雅黑',arail;
  cursor: pointer;
  background-color: #50bfff;
    color: #fff;
}
    
</style>