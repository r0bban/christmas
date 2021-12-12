<template>
  <div class="login">
    <form class="form" onsubmit="return false">
      <h1 class="header" for>Logga in</h1>
      <h3 v-if="authError">{{authError}}</h3>
      <div class="formRow">
        <label class="label" for="username">Användare: </label>
        <input
          class="input"
          v-model="creds.username"
          type="text"
          id="username"
          placeholder="användarnamn"
        />
      </div>
      <div class="formRow">
        <label class="label" for="password">Lösenord: </label>
        <input
          class="input"
          v-model="creds.password"
          type="password"
          id="password"
        />
      </div>
      <div class="std-btn">
        <button @click="login">Logga in</button>
      </div>
    </form>
    <p>{{this.getEmail}}</p>
    <form class="form" onsubmit="return false">
      <h1 class="header" for>Aktivera spel</h1>
      <h3 v-if="authError">{{authError}}</h3>
      <div class="formRow">
        <label class="label" for="username">Aktivera spel: </label>
        <input
            class="input"
            v-model="gameId"
            type="text"
            id="gameId"
            placeholder="gameId"
        />
      </div>
      <div class="std-btn">
        <button @click="finalizeGame">Slutför</button>
      </div>
    </form>
  </div>
</template>

<script>
import christmasAPI from "@/api/christmas";

export default {
  name: "Login",
  data() {
    return {
      creds: {
        username: "",
        password: "",
      },
      gameId: ""
    };
  },
  methods: {
    login() {
      this.$store.dispatch("authUser", this.creds);
    },
    async finalizeGame(){
      try {
        const response = await christmasAPI.finalizeGame(this.gameId, this.$store.state.authModule.accessToken);
          this.$dtoast.pop({
            color: 'black',
            background: 'orange',
            heading: `Response status`,
            content: response.status.toString(),
          })
      } catch (error) {
        this.$dtoast.pop({
          heading: `Error status`,
          content: error.toString(),
        })      }
    }
  },
  computed: {
    getEmail() {
      return this.$store.state.authModule.currentUser
      ? this.$store.state.authModule.currentUser.email
      : "ej inlogad";
    },
    authError() {
      let authError = this.$store.state.authModule.authErrorCode;
      if(authError == 1) return "Fel användarnamn lösenord eller lösenord";
      if(authError == 2) return "Din session har löpt, vänligen logga in för att bekräfta access.";
      return null;
    },
  },
};
</script>

<style scoped>
.login {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: lightgrey;
  position: absolute;
  top: 0;
  width: 100%;
  height: 100%;
}
</style>