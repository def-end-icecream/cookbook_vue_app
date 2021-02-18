<template>
  <div class="recipes-index">
    <div>Seach: <input type="text" v-model="filter" /></div>
    <div v-for="recipe in filterBy(recipes, filter)" v-bind:key="recipe.id">
      <h2>Title: {{ recipe.title }}</h2>
      <router-link :to="`/recipes/${recipe.id}`">
        <img v-bind:src="recipe.image_url" alt="" />
      </router-link>
      <p>Ingredients: {{ recipe.ingredients }}</p>
      <p>Directions: {{ recipe.directions }}</p>
      <p>Prep Time: {{ recipe.prep_time }}</p>
      <p>Created {{ relativeDate(recipe.created_at) }}</p>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
import moment from "moment";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      recipes: [],
      filter: "",
    };
  },
  created: function() {
    axios.get("/api/recipes").then((response) => {
      console.log(response.data);
      this.recipes = response.data;
    });
  },
  methods: {
    relativeDate: function(date) {
      return moment(date).fromNow();
    },
  },
};
</script>
