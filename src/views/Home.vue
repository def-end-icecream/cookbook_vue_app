<template>
  <div class="home">
    <h1>All Recipes</h1>
    <div v-for="recipe in recipes">
      <h2>Title: {{ recipe.title }}</h2>
      <p>Prep Time: {{ recipe.prep_time }}</p>
    </div>
  </div>
</template>

<style></style>

<script>
export default {
  data: function() {
    return {
      message: "Hello World!",
      recipes: [
        {
          id: 1,
          title: "Raw Eggs",
          prep_time: 5,
        },
        {
          id: 2,
          title: "Empty Sandwich",
          prep_time: 10,
        },
        {
          id: 3,
          title: "Ice Cubes",
          prep_time: 120,
        },
      ],
    };
  },
  created: function() {},
  methods: {},
};
</script>
