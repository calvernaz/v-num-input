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
  model: {
    event: 'change',
  },
  props: {
    readonly: {
      type: Boolean,
      default: false
    },
    rounded: {
      type: Boolean,
      default: false
    },
    max: {
      type: Number,
      default: Infinity,
    },
    min: {
      type: Number,
      default: -Infinity,
    },
    value: {
      type: Number,
      default: NaN,
    },
  },
  data() {
    return {
      counter: 5
    };
  },
  created() {
    this.counter =  Math.min(this.max, Math.max(this.min, this.value))
  },
  methods: {
    increment(arg) {
      const oldValue = this.counter;

      const amount = (typeof arg !== 'number') ? 1 : arg;

      this.counter = Math.min(this.max, Math.max(this.min, this.counter + amount))

      this.$emit('change', this.counter, oldValue)
    },
    decrement(arg) {
      const oldValue = this.counter;

      const amount = (typeof arg !== 'number') ? 1 : arg;

      this.counter = Math.min(this.max, Math.max(this.min, this.counter - amount))

      this.$emit('change', this.counter, oldValue)
    }
  }
};
</script>

<template>
  <div class="v-num-input">
    <button @click="decrement" v-long-click="decrement" v-bind:class="{ round: rounded }">-</button>
    <p v-if="readonly" v-model="counter">{{ counter }}</p>
    <button @click="increment" v-long-click="increment" v-bind:class="{ round: rounded }">+</button>
  </div>
</template>

<style scoped>
:root {
  --primary-color: #41b883;
  --on-primary-color: white;

  --small-spacing: 12px;
  --normal-spacing: calc(var(--small-spacing) * 2);
}
button {
  border: 4px solid var(--button-border-color, var(--primary-color));
  padding: var(--small-spacing) var(--normal-spacing);
  transition: 0.25s ease-in-out all;
}

.v-num-input {
  align-items: center;
  display: flex;
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
