<template>
  <div class="container">
    <h1 class="title">Search Page</h1>
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
      <b-button type="submit" variant="primary" style="width:250px;" class="ml-5 w-75">Search</b-button>     
    </b-form>

  <b-container>
    <b-row >
      <b-col v-for="r in recipes" :key="r.metadata.Id">
        <RecipePreview class="recipePreview" :recipe="r" />
      </b-col>
    </b-row>
  </b-container>
  </div>
</template>

<script>
import intolerances from "../assets/intolerances";
import diets from "../assets/diets";
import cuisines from "../assets/cuisines";
import RecipePreview from "../components/RecipePreview";
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
    RecipePreview
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
      cuisines: [{ value: null, text: "", disabled: true }]

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
        
        console.log(response.data);
        const recipes = response.data;
        this.recipes = [];
        var size = Object.keys(response.data).length;
        console.log(size);
        for (let index = 1; index < size+1; index++) {
          this.recipes.push(recipes["Recipe "+ index]);
        }
        //this.recipes.push(...recipes);
        console.log(this.recipes);
      } catch (error) {
        console.log(error);
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
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  min-height: 400px;
}
</style>

