<template>
  <div id="app">
    <header id="fh5co-header" role="banner">
      <div class="container">
        <div class="header-inner">
          <h1>
            <router-link to="/">Cookbook</router-link>
          </h1>
          <nav role="navigation">
            <ul>
              <li><router-link to="/recipes">Recipes</router-link></li>
              <li><router-link to="/recipes/new">New Recipe</router-link></li>
              <li>
                <router-link v-if="loggedIn()" to="/logout">Logout</router-link>
              </li>
              <li>
                <router-link v-if="!loggedIn()" to="/login">Login</router-link>
              </li>
              <li class="cta">
                <router-link v-if="!loggedIn()" to="/signup"
                  >Signup</router-link
                >
              </li>
            </ul>
          </nav>
        </div>
      </div>
    </header>

    <div v-if="flashMessage">
      {{ flashMessage }} <button v-on:click="flashMessage = ''">Close</button>
    </div>
    <div class="container">
      <router-view />
    </div>

    <footer id="fh5co-footer" role="contentinfo">
      <div class="container">
        <div class="col-md-3 col-sm-12 col-sm-push-0 col-xs-12 col-xs-push-0">
          <h3>About Us</h3>
          <p>
            Far far away, behind the word mountains, far from the countries
            Vokalia and Consonantia, there live the blind texts.
          </p>
          <p>
            <a href="#" class="btn btn-primary btn-outline with-arrow btn-sm"
              >Join Us <i class="icon-arrow-right"></i
            ></a>
          </p>
        </div>
        <div
          class="col-md-6 col-md-push-1 col-sm-12 col-sm-push-0 col-xs-12 col-xs-push-0"
        >
          <h3>Our Services</h3>
          <ul class="float">
            <li><a href="#">Web Design</a></li>
            <li><a href="#">Branding &amp; Identity</a></li>
            <li><a href="#">Free HTML5</a></li>
            <li><a href="#">HandCrafted Templates</a></li>
          </ul>
          <ul class="float">
            <li><a href="#">Free Bootstrap Template</a></li>
            <li><a href="#">Free HTML5 Template</a></li>
            <li><a href="#">Free HTML5 &amp; CSS3 Template</a></li>
            <li><a href="#">HandCrafted Templates</a></li>
          </ul>
        </div>

        <div
          class="col-md-2 col-md-push-1 col-sm-12 col-sm-push-0 col-xs-12 col-xs-push-0"
        >
          <h3>Follow Us</h3>
          <ul class="fh5co-social">
            <li>
              <a href="#"><i class="icon-twitter"></i></a>
            </li>
            <li>
              <a href="#"><i class="icon-facebook"></i></a>
            </li>
            <li>
              <a href="#"><i class="icon-google-plus"></i></a>
            </li>
            <li>
              <a href="#"><i class="icon-instagram"></i></a>
            </li>
          </ul>
        </div>
      </div>
    </footer>
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
