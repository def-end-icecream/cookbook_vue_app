<template>
  <div class="recipes-new">
    <p v-if="!$parent.loggedIn()">You are not logged in!</p>
    <form v-on:submit.prevent="createRecipe()">
      <h1>New Recipe</h1>
      <img v-if="status" :src="`https://http.cat/${status}`" alt="" />
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
      <div class="form-group">
        <label>Title:</label>
        <input type="text" class="form-control" v-model="title" />
      </div>
      <div class="form-group">
        <label>Ingredients:</label>
        <input type="text" class="form-control" v-model="ingredients" />
      </div>
      <div class="form-group">
        <label>Directions:</label>
        <input type="text" class="form-control" v-model="directions" />
      </div>
      <div class="form-group">
        <label>Prep Time:</label>
        <input type="text" class="form-control" v-model="prepTime" />
      </div>
      <div class="form-group">
        <label>Image Url:</label>
        <input type="text" class="form-control" v-model="imageUrl" />
      </div>
      <input type="submit" class="btn btn-primary" value="Create" />
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      title: "",
      ingredients: "",
      directions: "",
      prepTime: "",
      imageUrl: "",
      errors: [],
      status: "",
    };
  },
  methods: {
    createRecipe: function() {
      var params = {
        title: this.title,
        ingredients: this.ingredients,
        directions: this.directions,
        prep_time: this.prepTime,
        image_url: this.imageUrl,
      };
      axios
        .post("/api/recipes", params)
        .then((response) => {
          console.log(response.data);
          this.$router.push("/recipes");
        })
        .catch((error) => {
          this.status = error.response.status;
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
