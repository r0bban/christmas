<template>
  <main class="game-background">
    <div class="shade">
      <h1 class="header">{{ gameName ? gameName : "Julklappsbyte 2021" }}</h1>
      <section class="wrapper">
        <TokenForm v-if="this.showVerifyToken" :token="this.token" @onVerify="onVerify"/>
        <img src="@/assets/loader.png" v-if="this.loading"/>
        <div
            v-if="this.participantExist && !this.loading"
            class="participant-container"
        >
          <h2>Hej {{ participantName }}!</h2>
          <div>
            <span class="dymo">Här ser du vem du ska köpa julklapp till.</span>
          </div>
          <div style="margin: 10px 0">
            <span class="dymo">Julklappen ska kosta mellan 280-320 kr.</span>
          </div>
          <article class="recipient-container">
            <div v-if="this.showRecipient" class="recipient">
              <div>
                <h3 v-if="toBuyToExist">Grattis, du ska köpa julklapp till:</h3>
              </div>
              <div class="recipient-name">
                <h2 class="exist" v-if="toBuyToExist">{{ toBuyToName }}</h2>
                <h2 v-else>Oh nej, ej klart ännu 😢</h2>
              </div>
            </div>
          </article>
          <Btn fullWidth v-if="!this.showRecipient" @click="this.toggleShowRecipient">
            Se julklappsmottagare
          </Btn>
          <Btn
              v-if="this.showRecipient && toBuyToExist"
              @click="this.toggleShowRecipient"
              fullWidth
          >
            Dölj
          </Btn>
          <Btn fullWidth v-if="this.showRecipient && !toBuyToExist" @click="verify">
            Kolla igen
          </Btn>
          <Btn m="10px 0 0 0" fullWidth secondary @click="clear">Rensa kod från minnet</Btn>
        </div>
      </section>
    </div>
  </main>
</template>

<script>
import Btn from "../components/Btn";
import TokenForm from "../components/TokenForm";

export default {
  name: "ParticipantNEW",
  components: {TokenForm, Btn},
  data() {
    return {
      token: "",
      identifierad: false,
      showRecipient: false,
      loading: false,
    };
  },
  props: {
    participantToken: String,
  },
  created() {
    console.log("Params --->" + JSON.stringify(this.$route.params))
    if (this.participantToken) {
      this.token = this.participantToken;
      this.verify();
    }
  },
  methods: {
    onVerify(token) {
      this.token = token
      this.verify()
    },
    async verify() {
      this.loading = true;
      await this.$store.dispatch("authParticipant", this.token);
      this.loading = false;
    },
    toggleShowRecipient() {
      this.showRecipient = !this.showRecipient;
    },
    clear() {
      this.token = null;
      this.$store.dispatch("resetParticipant");
    },
    addToast(text) {
      this.$dtoast.pop({
        heading: `Felmeddelande`,
        content: text,
      })
    }
  },
  computed: {
    currentError() {
      return this.$store.state.christmasModule.participantError;
    },
    showVerifyToken() {
      return !this.participantExist && !this.loading;
    },
    participantExist() {
      return this.$store.state.christmasModule.currentParticipant
          ? this.$store.state.christmasModule.currentParticipant.name
          : false;
    },
    participantName() {
      return this.$store.state.christmasModule.currentParticipant
          ? this.$store.state.christmasModule.currentParticipant.name
          : "";
    },
    toBuyToExist() {
      return !!this.$store.state.christmasModule.currentParticipant
          .buyingToParticipant;
    },
    toBuyToName() {
      return this.$store.state.christmasModule.currentParticipant
          ? this.$store.state.christmasModule.currentParticipant
              .buyingToParticipant
          : "";
    },
    gameName() {
      return this.$store.state.christmasModule.currentParticipant
          ? this.$store.state.christmasModule.currentParticipant.giftGame
          : "";
    },
  },
  watch: {
    currentError(newValue) {
      if (newValue != null) {
        this.addToast(newValue);
        this.$store.commit("clearErrorMessage");
      }
    }
  },
};
</script>
<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css2?family=Lobster&display=swap');

.header {
  font-family: 'Lobster', cursive;
  text-align: center;
  font-style: italic;
  color: rgb(150, 12, 40);
  text-shadow: 1px 1px 2px white, 0px 0px 20px white; //rgb(150, 12, 40);
  margin: 0;
}

h2, h3, p {
  font-style: italic;
  color: white;
  text-shadow: 1px 1px 2px black, 0px 0px 20px black; //rgb(150, 12, 40);
  margin: 0;
}

h2 {
  font-size: 30px;
  margin-bottom: 15px;
  font-family: 'Arial', cursive;

}

p {
  font-size: 18px;
}

.dymo {
  border-top: 1px solid #6c5c5d;
  padding: 2px 10px 4px 10px;
  line-height: 25px;
  font-size: 15px;
  background: linear-gradient(to bottom, #31221f, #030000);
  color: #c3bcb6;
  font-family: sans-serif;
  text-shadow: -1px -1px 0 #030000,
  1px -1px 0 #030000,
  -1px 1px 0 #030000,
  1px 1px 0 #030000,
  0 0 4px #fff;
  letter-spacing: 3px;
  text-transform: uppercase;
  font-family: courier;
  font-weight: bold;
  -webkit-box-decoration-break: clone;
  box-decoration-break: clone
}

.game-background {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100%;
  background: rgb(150, 12, 40) url("../assets/bg-photo.jpg") center;
  background-size: cover;
  box-sizing: border-box;
  overflow: auto;

  .shade {
    display: inherit;
    flex-direction: inherit;
    align-items: inherit;
    box-sizing: inherit;
    overflow: inherit;
    width: 100%;
    height: 100%;
    background-color: rgba(255, 255, 255, 0.2);
  }

  .wrapper {
    display: inherit;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 100%;
    max-width: 400px;
    flex-grow: 1;
    box-sizing: border-box;
  }
}

.participant-container {
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  width: 100%;
  height: 100%;
  min-height: 100%;
  padding: 20px;
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  color: #fff;
}

.recipient-container {
  text-align: center;
  min-height: 120px;
  margin-bottom: 20px;

  .recipient {
    display: flex;
    flex-direction: column;
    //background: #2c3e50;
    height: 100%;

    .recipient-name {
      display: flex;
      flex-grow: 1;
      flex-direction: column;
      margin: 0;
      justify-content: center;

      .exist {
        text-transform: uppercase;
      }
    }
  }


}
</style>