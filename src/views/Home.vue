<template>
  <div class="home">
    <h1>New Recipe</h1>
    <div>
      <p v-for="error in createErrors">{{ error }}</p>
      Title: <input type="text" v-model="newRecipeTitle" /> <br />
      Ingredients: <input type="text" v-model="newRecipeIngredients" /><br />
      Directions: <input type="text" v-model="newRecipeDirections" /><br />
      Prep Time: <input type="text" v-model="newRecipePrepTime" /><br />
      Image Url: <input type="text" v-model="newRecipeImageUrl" /><br />
      <button v-on:click="createRecipe()">Create</button>
    </div>

    <h1>All Recipes</h1>
    <div v-for="recipe in recipes" v-bind:key="recipe.id">
      <h2>Title: {{ recipe.title }}</h2>
      <img v-bind:src="recipe.image_url" alt="" />
      <p>Ingredients: {{ recipe.ingredients }}</p>
      <p>Directions: {{ recipe.directions }}</p>
      <p>Prep Time: {{ recipe.prep_time }}</p>
      <button v-on:click="showRecipe(recipe)">More Info</button>
    </div>

    <dialog>
      <form method="dialog">
        <h2>Recipe Info</h2>
        <img :src="currentRecipe.image_url" alt="" />
        <p>Title: <input type="text" v-model="currentRecipe.title" /></p>
        <p>
          Ingredients: <input type="text" v-model="currentRecipe.ingredients" />
        </p>
        <p>
          Directions: <input type="text" v-model="currentRecipe.directions" />
        </p>
        <p>
          Prep Time: <input type="text" v-model="currentRecipe.prep_time" />
        </p>
        <button v-on:click="updateRecipe()">Update</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style>
img {
  width: 250px;
}
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      recipes: [],
      newRecipeTitle: "",
      newRecipeIngredients: "",
      newRecipeDirections: "",
      newRecipePrepTime: "",
      newRecipeImageUrl: "",
      currentRecipe: {},
      createErrors: [],
    };
  },
  created: function() {
    this.indexRecipes();
  },
  methods: {
    indexRecipes: function() {
      axios.get("/api/recipes").then((response) => {
        console.log(response.data);
        this.recipes = response.data;
      });
    },
    createRecipe: function() {
      var params = {
        title: this.newRecipeTitle,
        ingredients: this.newRecipeIngredients,
        directions: this.newRecipeDirections,
        prep_time: this.newRecipePrepTime,
        image_url: this.newRecipeImageUrl,
      };
      axios
        .post("/api/recipes", params)
        .then((response) => {
          console.log(response.data);
          this.recipes.push(response.data);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
          this.createErrors = error.response.data.errors;
        });
    },
    showRecipe: function(inputRecipe) {
      console.log(inputRecipe);
      this.currentRecipe = inputRecipe;
      document.querySelector("dialog").showModal();
    },
    updateRecipe: function() {
      var params = {
        title: this.currentRecipe.title,
        ingredients: this.currentRecipe.ingredients,
        directions: this.currentRecipe.directions,
        prep_time: this.currentRecipe.prep_time,
        image_url: this.currentRecipe.image_url,
      };
      axios
        .patch(`/api/recipes/${this.currentRecipe.id}`, params)
        .then((response) => {
          console.log("Success", response.data);
        })
        .catch((error) => {
          console.log("Error", error.response.data);
        });
    },
  },
};
</script>
