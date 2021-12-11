<template>
  <div class="tokenFormWrapper">
    <form class="form" onsubmit="return false">
      <div class="form-row">
        <input
            class="input"
            v-model="inputToken"
            type="text"
            id="token"
            placeholder="ange kod"
        />
      </div>
      <div class="std-btn">
        <Btn fullWidth :disabled="!inputToken" @click="callback()">
          Verifiera
        </Btn>
      </div>
    </form>
  </div>
</template>

<script>
import Btn from "../components/Btn";

export default {
  name: "TokenForm",
  components: { Btn },
  data() {
    return {
      inputToken: this.token
    };
  },
  props: {
    token: String,
    onSubmit: Function
  },
  created() {
  },
  methods: {
    callback: function () {
      this.$emit('onVerify', this.inputToken);
    },
  },
  computed: {
  },
  watch: {
  },
};
</script>

<style lang="scss" scoped>
.tokenFormWrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  justify-items: center;
  top: 0;
  width: 250px;
  height: 100%;

  .form {
    width: 100%;

    .form-row {
      display: inherit;
      flex-direction: column;
      margin: 15px 0;
      width: inherit;

      .input {
        color: black;
        font-size: 1.2em;
        border: 0;
        border-radius: 3px;
        text-align: center;
        height: 30px;
        width: inherit;
        box-sizing: border-box;
      }
    }
  }
}
</style>