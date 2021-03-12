<template>
  <div class="recipes-index">
    <datalist id="titles">
      <option v-for="recipe in recipes" v-bind:key="recipe.title">{{
        recipe.title
      }}</option>
    </datalist>
    <!-- <div class="jumbotron jumbotron-fluid">
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
    </div> -->

    <div id="fh5co-blog-section" class="fh5co-light-grey-section">
      <div class="container">
        <div class="row">
          <div class="col-md-6 col-md-offset-3 text-center fh5co-heading">
            <h2>Browse Recipes</h2>
            <p>
              They are so delicious!
            </p>
          </div>
        </div>
        <div class="row">
          <div
            class="col-md-6 col-sm-6"
            v-for="recipe in orderBy(filterBy(recipes, filter), sortAttribute)"
            v-bind:key="recipe.id"
          >
            <router-link :to="`/recipes/${recipe.id}`" class="item-grid">
              <div
                class="image"
                :style="`background-image: url(${recipe.image_url})`"
              ></div>
              <div class="v-align">
                <div class="v-align-middle">
                  <h3 class="title">{{ recipe.title }}</h3>
                  <h5 class="date">
                    <span>Created {{ relativeDate(recipe.created_at) }}</span>
                  </h5>
                  <!-- <p>Ingredients: {{ recipe.ingredients }}</p> -->
                </div>
              </div>
            </router-link>
          </div>

          <div class="col-md-12 text-center animate-box">
            <p>
              <a href="#" class="btn btn-primary with-arrow"
                >View More Post <i class="icon-arrow-right"></i
              ></a>
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

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
