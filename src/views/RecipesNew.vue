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
        <input
          type="file"
          class="form-control"
          v-on:change="setFile($event)"
          ref="fileInput"
        />
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
      imageFile: "",
      errors: [],
      status: "",
    };
  },
  methods: {
    setFile: function(event) {
      if (event.target.files.length > 0) {
        this.imageFile = event.target.files[0];
      }
    },
    createRecipe: function() {
      var formData = new FormData();
      formData.append("title", this.title);
      formData.append("ingredients", this.ingredients);
      formData.append("directions", this.directions);
      formData.append("prep_time", this.prepTime);
      formData.append("image_file", this.imageFile);
      axios
        .post("/api/recipes", formData)
        .then((response) => {
          console.log(response.data);
          this.$parent.flashMessage = "Recipe successfully created, good job!";
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
