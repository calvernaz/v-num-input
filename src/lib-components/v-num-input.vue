<script>

import longclick from "@/lib-components/directives/longclick";

export default {
  name: 'VNumInput', // vue component name
  directives: {
    longclick
  },
  props: {
    readonly: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      counter: 5,
      initCounter: 5,
      message: {
        action: null,
        amount: null,
      },
    };
  },
  computed: {
    changedBy() {
      const { message } = this;
      if (!message.action) return 'initialized';
      return `${message?.action} ${message.amount ?? ''}`.trim();
    },
  },
  methods: {
    increment(arg) {
      const amount = (typeof arg !== 'number') ? 1 : arg;
      this.counter += amount;
      this.message.action = 'incremented by';
      this.message.amount = amount;
    },
    decrement(arg) {
      const amount = (typeof arg !== 'number') ? 1 : arg;
      this.counter -= amount;
      this.message.action = 'decremented by';
      this.message.amount = amount;
    },
    reset() {
      this.counter = this.initCounter;
      this.message.action = 'reset';
      this.message.amount = null;
    },
  },
};
</script>

<template>
  <div class="v-num-input">
    <button v-longclick="decrement">-</button>

    <p v-if="readonly">{{ counter }}</p>

    <button v-longclick="increment">+</button>
  </div>
</template>

<style scoped>
  .v-num-input {
    display: flex;
  }
  .v-num-input p {
    padding: 0.20rem;
  }
</style>
