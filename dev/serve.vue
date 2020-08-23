<script>
import Vue from 'vue';
import { VNumInput } from '@/entry';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    VNumInput,
  },
  methods: {
    onchange: function (n, o) {
      console.log('onchange', n, o);
    }
  }
});
</script>

<template>
  <div id="app">
    <v-num-input class="custom-theme" readonly rounded :min="0" :max="5" v-on:change="onchange" :value="-1"/>
  </div>
</template>

<style scoped>
.custom-theme {
  --button-border-color: pink;
  --button-hover-background-color: rgb(206, 161, 195);
}
</style>
