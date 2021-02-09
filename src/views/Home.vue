<template>
  <div class="home">
    <h1>New Recipe</h1>
    <button v-on:click="createRecipe()">Create</button>
    <h1>All Recipes</h1>
    <div v-for="recipe in recipes" v-bind:key="recipe.id">
      <h2>Title: {{ recipe.title }}</h2>
      <img v-bind:src="recipe.image_url" alt="" />
      <p>Ingredients: {{ recipe.ingredients }}</p>
      <p>Directions: {{ recipe.directions }}</p>
      <p>Prep Time: {{ recipe.prep_time }}</p>
    </div>
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
      message: "Hello World!",
      recipes: [],
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
        title: "Roasted Brussel Sprouts",
        ingredients: "Brussels, olive oil, salt, pepper",
        directions: "Roast the brussel sprouts",
        prep_time: 20,
        image_url:
          "https://food.fnr.sndimg.com/content/dam/images/food/fullset/2010/8/12/0/FN-Thanksgiving-2010_Brussels-Sprouts_s4x3.jpg.rend.hgtvcom.826.620.suffix/1384540892898.jpeg",
      };
      axios.post("/api/recipes", params).then((response) => {
        console.log(response.data);
      });
    },
  },
};
</script>
