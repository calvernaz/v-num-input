<script>

export default {
  name: 'VNumInput', // vue component name
  directives: {
    longClick: {
      bind: function (el, binding, vNode) {
        let interval = 50;
        let delay = 400;

        if (typeof binding.value !== 'function') {
          const compName = vNode.context.name
          let warn = `[longclick:] provided expression '${binding.expression}' is not a function, but has to be`
          if (compName) {
            warn += `Found in component '${compName}' `
          }
          console.warn(warn) // eslint-disable-line
        }

        let pressTimer = null
        let pressInterval = null

        const start = (e) => {
          if (e.type === 'click' && e.button !== 0) {
            return
          }

          if (pressTimer === null) {
            pressTimer = setTimeout(() => {
              if (interval && interval > 0) {
                pressInterval = setInterval(() => {
                  handler()
                }, interval)
              }
              handler()
            }, delay)
          }
        }

        // Cancel Timeout
        const cancel = () => {
          if (pressTimer !== null) {
            clearTimeout(pressTimer)
            pressTimer = null
          }
          if (pressInterval) {
            clearInterval(pressInterval)
            pressInterval = null
          }
        }
        // Run Function
        const handler = (e) => {
              binding.value(e)
            }

        ;['mousedown', 'touchstart'].forEach(e => el.addEventListener(e, start))
        ;['click', 'mouseout', 'touchend', 'touchcancel'].forEach(e => el.addEventListener(e, cancel))
      }
    },
  },
  props: {
    readonly: {
      type: Boolean,
      default: false
    },
    rounded: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      counter: 5,
      initCounter: 5,
    };
  },
  methods: {
    increment(arg) {
      const amount = (typeof arg !== 'number') ? 1 : arg;
      this.counter += amount;
    },
    decrement(arg) {
      const amount = (typeof arg !== 'number') ? 1 : arg;
      this.counter -= amount;
    }
  }
};
</script>

<template>
  <div class="v-num-input">
    <button @click="decrement" v-long-click="decrement" v-bind:class="{ round: rounded }">-</button>
    <p v-if="readonly">{{ counter }}</p>
    <button @click="increment" v-long-click="increment" v-bind:class="{ round: rounded }">+</button>
  </div>
</template>

<style scoped>
button {
  width: 30px;
  height: 30px;
}

.v-num-input {
  display: flex;
  align-items: center;
  justify-content: center;
}

.v-num-input p {
  padding: 0.20rem;
  margin: 0 5px;
}

.v-num-input button {
  outline: none;
}

.v-num-input button.round {
  border-radius: 50%;

}
</style>
