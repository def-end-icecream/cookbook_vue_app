<template>
  <div class="recipes-show">
    <h1>{{ recipe.title }}</h1>
    <img :src="recipe.image_url" alt="" />
    <p>Ingredients: {{ recipe.ingredients }}</p>
    <p>Directions: {{ recipe.directions }}</p>
    <p>Prep Time: {{ recipe.prep_time }}</p>
    <router-link :to="`/recipes/${recipe.id}/edit`"
      ><button>Edit</button></router-link
    >
    <button v-on:click="destroyRecipe()">Destroy</button>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      recipe: {},
    };
  },
  created: function() {
    axios.get(`/api/recipes/${this.$route.params.id}`).then((response) => {
      this.recipe = response.data;
      console.log(this.recipe);
    });
  },
  methods: {
    destroyRecipe: function() {
      // true when the user hits ok
      // false when the user hits cancel
      if (confirm("Are you sure you want to delete this recipe?")) {
        axios.delete(`/api/recipes/${this.recipe.id}`).then((response) => {
          console.log(response.data);
          this.$router.push("/recipes");
        });
      }
    },
  },
};
</script>
