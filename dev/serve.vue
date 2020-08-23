<script>
import Vue from 'vue';
import { VNumInput } from '@/entry';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    VNumInput,
  }
});
</script>

<template>
  <div id="app">
    <v-num-input readonly rounded/>
  </div>
</template>
