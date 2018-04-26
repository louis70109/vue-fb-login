<template>
  <div id="app">
    <hello  :profile="profile"></hello>
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-2">
          <button type="button" class="btn btn-outline-success" @click="login">Login</button>
        </div>
        <div class="col-2">
          <button type="button" class="btn btn-outline-success" @click="logout">Logout</button>
        </div>
        <div class="col-2">
          <button type="button" class="btn btn-outline-success" @click="getProfile">getfile</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Hello from "./components/HelloWorld";
import Vue from "vue";
import BootstrapVue from "bootstrap-vue";
Vue.use(BootstrapVue);
export default {
  name: "App",
  components: {
    Hello
  },
  data() {
    return {
      profile: {},
      authorized: false
    };
  },
  methods: {
    getProfile() {
      FB.api("/me?fields=name,id,email", function(response) {
        console.log("res in graphAPI", response);
      });
    },
    logout() {
      let vm = this;
      FB.logout(function(response) {
        console.log("res when logout", response);
      });
    },
    login() {
      let vm = this;
      FB.login(
        function(response) {
          console.log("res", response);
        },
        {
          scope:
            "public_profile,user_friends,email,user_location,user_photos,user_status,user_tagged_places,user_videos",
          return_scopes: true
        }
      );
    },
    statusChangeCallback(response) {
      let vm = this;
      if (response.status === "connected") {
        vm.authorized = true;
        vm.getProfile();
      } else if (response.status === "not_authorized") {
        vm.authorized = false;
      } else {
        vm.authorized = false;
      }
    }
  },
  mounted() {
    let vm = this;

    window.fbAsyncInit = function() {
      FB.init({
        appId: "201944310209098",
        cookie: true,
        xfbml: true,
        version: "v2.12"
      });
      FB.AppEvents.logPageView();
      // Get FB Login Status
      FB.getLoginStatus(response => {
        vm.statusChangeCallback(response);
      });
    };
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
