<template>

  <div class="container">
        <p><br></p>
    <div>
    <b-jumbotron header="Search" lead="Get the perfect recipe for you...">    </b-jumbotron>
    </div>
    <b-form @submit.prevent="onSubmit" @reset.prevent="onReset">

      <b-form-group
        id="input-group-query"
        label-cols-sm="3"
        label="Search Query:"
        label-for="query"
      >
        <b-form-input
          id="query"
          v-model= "form.query"
          type="text"
          :state="validateState('query')"
        ></b-form-input>
        <b-form-invalid-feedback v-if="!$v.form.query.required">
          query is required
        </b-form-invalid-feedback>
      </b-form-group>

      <b-form-group
        id="input-group-size"
        label-cols-sm="3"
        label="Size:"
        label-for="size"
      >
        <b-form-select
          id="size"
          v-model="form.size"
          :options="[5,10,15]"
        ></b-form-select>
      </b-form-group>

      <b-form-group
        id="input-group-intolerance"
        label-cols-sm="3"
        label="Intolerance:"
        label-for="intolerance"
      >
        <b-form-select
          id="intolerance"
          v-model="form.intolerance"
          :options="intolerances"
        ></b-form-select>
      </b-form-group>

      <b-form-group
        id="input-group-diet"
        label-cols-sm="3"
        label="Diet:"
        label-for="diet"
      >
        <b-form-select
          id="diet"
          v-model="form.diet"
          :options="diets"
        ></b-form-select>
      </b-form-group>

      <b-form-group
        id="input-group-cuisine"
        label-cols-sm="3"
        label="Cuisine:"
        label-for="cuisine"
      >
        <b-form-select
          id="cuisine"
          v-model="form.cuisine"
          :options="cuisines"
        ></b-form-select>
      </b-form-group>

      <b-button type="reset" variant="danger">Reset</b-button>
      <b-button type="submit" variant="primary" style="width:250px; float: right;" class="ml-5 w-75">Search</b-button>     
    </b-form>
    <b-alert
      class="mt-2"
      v-if="form.submitError"
      variant="warning"
      dismissible
      show
    >
      Search result are empty: {{ form.submitError }}
    </b-alert>
    <br>  
    <p><br></p>
  <b-container v-if="recipes.length != 0">
  <br>
    <!-- <h3>Hi {{ $root.store.username }}</h3> -->
    <b-jumbotron class="title w3-center w3-padding-4"  header="Search Results:"></b-jumbotron>
    <b-dropdown text="Sort by">
          <b-dropdown-item @click="sortPopularity">Popularity</b-dropdown-item>
          <b-dropdown-item @click="sortTime">Duration</b-dropdown-item>
    </b-dropdown>  
    <RecipePreviewList :recipes="recipes"/>
  </b-container>
  </div>
</template>

<script>
import intolerances from "../assets/intolerances";
import diets from "../assets/diets";
import cuisines from "../assets/cuisines";
import RecipePreviewList from "../components/RecipePreviewList";
import {
  required,
  minLength,
  maxLength,
  alpha,
  sameAs,
  email,
  helpers 
} from "vuelidate/lib/validators";

export default {  
  name: "search",
  components: {
    RecipePreviewList
  },  
  data() {
    return {
      form: {
        query: "",
        size: "5",
        intolerance: "",
        diet: "",
        cuisine: ""
      },
      recipes: [],
      intolerances: [{ value: null, text: "", disabled: true }],
      diets: [{ value: null, text: "", disabled: true }],
      cuisines: [{ value: null, text: "", disabled: true }],
      byPopularity: true
    };
  },
  validations: {
    form: {
      query: {
        required,
        alpha
      }
    }
  },
  mounted() {
    this.intolerances.push(...intolerances);
    this.diets.push(...diets);
    this.cuisines.push(...cuisines);
    if(sessionStorage.key("lastSearchParams")!=null){
      let tmpform = JSON.parse(sessionStorage.getItem("lastSearchParams"));
      this.form.query = tmpform.query;
      this.form.size = tmpform.size;
      this.form.intolerance = tmpform.intolerance;
      this.form.diet = tmpform.diet;
      this.form.cuisine = tmpform.cuisine;
    }
    if(sessionStorage.key("lastSearchResults")!=null){
      this.recipes = JSON.parse(sessionStorage.getItem("lastSearchResults"));
    }

  },
  beforeDestroy(){
    sessionStorage.setItem("lastSearchResults",JSON.stringify(this.recipes))
    sessionStorage.setItem("lastSearchParams",JSON.stringify(this.form))
  },
  methods: {
    validateState(param) {
      const { $dirty, $error } = this.$v.form[param];
      return $dirty ? !$error : null;
    },
    async onSubmit() {
      try {
        this.$v.form.$touch();
          if (this.$v.form.$anyError) {
          return;
        }
        const response = await this.axios.get(
          `http://localhost:3000/search/${this.form.query}/?SearchSize=${this.form.size}&Intolerance=${this.form.intolerance}&Diet=${this.form.diet}&Cuisine=${this.form.cuisine}`  
        );
        var size = Object.keys(response.data).length;
        if(size===0){this.form.submitError = "No results for given parameters."}
        const recipes = response.data;
        this.recipes = [];
        for (let index = 1; index < size+1; index++) {
          this.recipes.push(recipes["Recipe "+ index]);
        }
        //this.recipes.push(...recipes);
        this.sort();
      } catch (err) {
        this.form.submitError = err.response.data.message;
      }
    },
    onReset() {
      this.form = {
        query: "",
        size: "5",
        intolerance: "",
        diet: "",
        cuisine: ""
      };
      this.$nextTick(() => {
        this.$v.$reset();
      });
    },
    resort(){
      this.byPopularity=!this.byPopularity;
      this.sort()
    },
    sort(){
      if(this.byPopularity){
        this.sortPopularity()
      }else{
        this.sortTime()
      }

    },
    sortPopularity(){
      this.byPopularity=true;
      this.recipes.sort(function(b, a) {
        return parseFloat(a.Popularity) - parseFloat(b.Popularity);
      });
    },
    sortTime(){
      this.byPopularity=false;
      this.recipes.sort(function(a, b) {
        return parseFloat(a.Time) - parseFloat(b.Time);
      });
    }         
  },
  
};
</script>

<style lang="scss" scoped>
.container {
  background-color:#c0c2c285;
}
.jumbotron {
  background-color:#f0f7f7bb;
  padding: 1rem 2rem;
  text-align: center;
}
form{
  width: 70%;
  margin: auto;
}
</style>

