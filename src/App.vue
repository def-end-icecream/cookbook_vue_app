<template>
  <div id="app">
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <router-link class="navbar-brand" to="/">Cookbook</router-link>
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarNav"
        aria-controls="navbarNav"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item">
            <router-link class="nav-link" to="/recipes">Recipes</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/recipes/new"
              >New Recipe</router-link
            >
          </li>
          <li class="nav-item">
            <router-link class="nav-link" v-if="!loggedIn()" to="/signup"
              >Signup</router-link
            >
          </li>
          <li class="nav-item">
            <router-link class="nav-link" v-if="!loggedIn()" to="/login"
              >Login</router-link
            >
          </li>
          <li class="nav-item">
            <router-link class="nav-link" v-if="loggedIn()" to="/logout"
              >Logout</router-link
            >
          </li>
        </ul>
      </div>
    </nav>

    <div v-if="flashMessage">
      {{ flashMessage }} <button v-on:click="flashMessage = ''">Close</button>
    </div>
    <div class="container">
      <router-view />
    </div>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      flashMessage: "",
    };
  },
  methods: {
    loggedIn: function() {
      return localStorage.jwt ? true : false;
    },
    getUserId: function() {
      return localStorage.user_id;
    },
  },
};
</script>
