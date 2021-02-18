<template>
  <div class="recipes-index">
    <div>
      Search: <input type="text" v-model="filter" list="titles" />
      <datalist id="titles">
        <option v-for="recipe in recipes" v-bind:key="recipe.title">{{
          recipe.title
        }}</option>
      </datalist>
    </div>
    <div>
      <button v-on:click="sortAttribute = 'prep_time'">
        Sort by prep time
      </button>
      <button v-on:click="sortAttribute = 'title'">Sort by title</button>
    </div>
    <div
      v-for="recipe in orderBy(filterBy(recipes, filter), sortAttribute)"
      v-bind:key="recipe.id"
    >
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
      sortAttribute: "title",
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
