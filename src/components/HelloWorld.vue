<template>
<div class="hello">

  <form class="search-form">
      <img v-show="profile.name" 
          :src="profilePicture" alt="profile" class="profile-picture"/>
      <h1 v-html="msg"></h1>
    <input type="text" class="font-color" v-model="filter">
    
    <div v-if="filter && filterArray.length">
      <div v-for="city in filterArray">
        <div>{{city.city}}, {{city.state}}</div>
      </div>
    </div>
    <p>{{filter}}</p>
  </form>
</div>

</template>

<script>
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";
Vue.use(VueAxios, axios);

export default {
  props: ["profile"],
  name: "hello",
  data() {
    return {
      cities: null,
      filter: ""
    };
  },
  computed: {
    regexp() {
      return new RegExp(this.filter, "gi");
    },
    filterArray() {
      return this.cities.filter(
        city => city.city.match(this.regexp) || city.state.match(this.regexp)
      );
    },
    msg() {
      console.log(`aaaaa${this.profile}`);
      if (this.profile.name) {
        return `Welcome <b><i> ${this.profile.name} </i></b> to Vue.js App`;
      } else {
        return "Login Facebook to Enjoy the App";
      }
    },
    profilePicture() {
      return this.profile.id
        ? `https://graph.facebook.com/${this.profile.id}/picture?width=300`
        : `/static/man.gif`;
    }
  },
  mounted() {
    Vue.axios
      .get(
        "https://gist.githubusercontent.com/Miserlou/c5cd8364bf9b2420bb29/raw/2bf258763cdddd704f8ffd3ea9a3e81d25e2c6f6/cities.json"
      )
      .then(response => {
        this.cities = response.data;
      })
      .catch(response => {
        /* 失敗，發生錯誤，然後...*/
      })
      .finally(() => {
        /* 不論成功失敗，都做些事 */
      });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
$val: #42b983;
.font-color {
  color: $val;
}
</style>
