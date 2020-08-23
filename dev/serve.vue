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
    <v-num-input readonly :min="0" :max="5" v-on:change="onchange" :value="-1" class="custom-theme" />
  </div>
</template>

<style scoped>
.custom-theme {
  --normal-padding: 50px;
  --button-border-color: green;
}
</style>
