<template>
  <button
      class="btn"
      v-bind:class="{
      secondary: this.secondary,
      fullWidth: fullWidth,
  }"
      :disabled="this.loading"
     @click="callback($event)"
      v-bind:style="{ margin: m }"
  >
    <slot/>
  </button>
</template>

<script>
export default {
  name: "Btn",
  props: {
    secondary: Boolean,
    fullWidth: Boolean,
    loading: Boolean,
    onClick: Function,
    m: String
  },
  methods: {
    callback: function (e) {
      this.$emit('click', e);
    },
  },
}
</script>

<style scoped>
.btn {
  padding-left: 1.8rem;
  padding-right: 1.8rem;
  height: 3rem;
  border: 0;
  border-radius: 50px;
  color: black;
  background: white;
  font-weight: bolder;
  font-size: 1.3em;
}

.btn.secondary {
  /*margin-top: 20px;*/
  border: 2px solid gray;
  border-radius: 50px;
  color: gray;
  background: black;
}

.btn.fullWidth {
  width: 100%;
}
</style>