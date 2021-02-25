<template>
  <div class="recipes-index">
    <datalist id="titles">
      <option v-for="recipe in recipes" v-bind:key="recipe.title">{{
        recipe.title
      }}</option>
    </datalist>
    <div class="jumbotron jumbotron-fluid">
      <div class="container">
        <h1 class="display-4">All Recipes</h1>
        <p class="lead">
          Search, sort, and browse!
        </p>
        <div class="row justify-content-md-center">
          <div
            class="btn-toolbar mb-3"
            role="toolbar"
            aria-label="Toolbar with button groups"
          >
            <div class="btn-group mr-2" role="group" aria-label="First group">
              <button
                v-on:click="sortAttribute = 'prep_time'"
                class="btn btn-secondary"
              >
                Sort prep time
              </button>
              <button
                v-on:click="sortAttribute = 'title'"
                class="btn btn-secondary"
              >
                Sort title
              </button>
            </div>
            <div class="input-group">
              <input
                type="text"
                class="form-control"
                placeholder="Search"
                v-model="filter"
                aria-describedby="btnGroupAddon"
              />
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="row row-cols-1 row-cols-md-3">
      <div
        class="col mb-4"
        v-for="recipe in orderBy(filterBy(recipes, filter), sortAttribute)"
        v-bind:key="recipe.id"
      >
        <div class="card h-100">
          <img v-bind:src="recipe.image_url" class="card-img-top" alt="..." />
          <div class="card-body">
            <h5 class="card-title">{{ recipe.title }}</h5>
            <p class="card-text">Ingredients: {{ recipe.ingredients }}</p>
            <p class="card-text">
              <small class="text-muted"
                >Created {{ relativeDate(recipe.created_at) }}</small
              >
            </p>
            <router-link class="btn btn-primary" :to="`/recipes/${recipe.id}`"
              >See recipe</router-link
            >
          </div>
        </div>
      </div>
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
