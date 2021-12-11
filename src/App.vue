<template>
  <div id="app">
    <!-- <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div> -->
    <router-view />
    <LoginComponent v-if="showLogin" />
  </div>
</template>
<script>
import LoginComponent from "@/components/LoginComponent.vue";
import Vue from "vue";
import 'dtoaster/dist/dtoaster.css'
import DToaster from 'dtoaster'

Vue.use(DToaster, {
  containerVerticalOffset: '200px',
  position: 'top-right', //toasts container position on the screen
  containerOffset: '45px', //toasts container offset from top/bottom of the screen
  color: 'white',
  background: 'red',
  progress_thumb_bg: 'white',
  duration: '5000',
  width: '250px'
})

export default {
  data() {
    return {};
  },
  components: {
    LoginComponent
  },
  methods: {
    // beforeCreate() {
    //   this.$store.dispatch("initialProductLoad");
    // },
  },
  computed: {
    showLogin() {
      return this.$store.state.showLogin;
    },
    participantError() {
      return this.$store.state.christmasModule.participantError;
    },
  },
  created() {
    console.log("created!");
    this.$store.dispatch("prepareUser");
    this.$store.dispatch("prepareParticipant");  
  },
};
</script>
<style lang="scss">
html, body {
  margin: 0;
  height: 100%;
  overflow: auto;
}

// body {
//   margin: 0;
//   min-height: 100%;
// }
//html {
//  height: 100%;
//  //overflow: hidden;
//  //width: 100%;
//  //height: -webkit-fill-available;
//  background: #42b983;
//  margin: 0;
//  padding: 0;
//}
//body {
//  height: 100%;
//  min-height: 100%;
//  max-height: fit-content;
//
//  //position: fixed;
//  //overflow-y: scroll;
//  //-webkit-overflow-scrolling: touch;
//  //min-height: 100vh;
//  //height: 100vh;
//  ///* mobile viewport bug fix */
//  //min-height: -webkit-fill-available;
//  //margin: 0;
//  padding: 10px;
//  margin: 0;
//  background: yellow;
//  //left: -10px;
//  box-sizing:border-box;
//}


#app {
  height: 100%;
  //min-height: 100%;
  margin: 0 auto;
  //width: 100%;
  //min-height: 100%;
  //height: 100%;
  // font-family: Avenir, Helvetica, Arial, sans-serif;
  // -webkit-font-smoothing: antialiased;
  // -moz-osx-font-smoothing: grayscale;
  // text-align: center;
  background: purple;
  box-sizing:border-box;
  overflow: auto;
  //padding: 10px;
  //box-sizing:border-box;
}

// #nav {
//   padding: 30px;

//   a {
//     font-weight: bold;
//     color: #2c3e50;

//     &.router-link-exact-active {
//       color: #42b983;
//     }
//   }
// }
</style>
