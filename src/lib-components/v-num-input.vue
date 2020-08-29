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
    this.counter = Math.min(this.max, Math.max(this.min, this.value))
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
    <button @click="decrement" v-long-click="decrement" class="minus"></button>
    <p v-if="readonly" v-model="counter">{{ counter }}</p>
    <button @click="increment" v-long-click="increment" class="plus"></button>
  </div>
</template>

<style scoped>

.v-num-input {
  align-items: center;
  display: flex;
  justify-content: center;

  --button-border-width: 2px;
  --button-border-style: solid;
  --button-border-color: green;
  --butotn-border-radius: 100%;
  --button-background-color: white;
  --button-plus-background: green;
  --button-minus-background: green;
  --button-width: 22px;
  --button-height: 22px;
  --button-margin: 0;
  --normal-padding: 0;
  --text-color: green;
}

.v-num-input button {
  border-width: var(--button-border-width);
  border-style: var(--button-border-style);
  border-color: var(--button-border-color);
  border-radius: var(--butotn-border-radius);
  background-color: var(--button-background-color);
  width: var(--button-width);
  height: var(--button-height);
  position: relative;
  margin: var(--button-margin);
  display: inline-block;
  vertical-align: middle;
  outline: none;
}

.v-num-input button:before,
.v-num-input button:after {
  content:'';
  position:absolute;
  top:0;
  left:0;
  right:0;
  bottom:0;
}

/* PLUS */

.v-num-input button.plus:before,
.v-num-input button.plus:after {
  background:var(--button-plus-background);
  /*box-shadow: 1px 1px 1px #ffffff9e;*/
}

.v-num-input button.plus:before{
  width: 2px;
  margin: 3px auto;
}

.v-num-input button.plus:after{
  margin: auto 3px;
  height: 2px;
  box-shadow: none;
}
/* MINUS */
.v-num-input button.minus:before{
  background: var(--button-minus-background);
  margin: auto 3px;
  height: 2px;
  /*box-shadow: 0 1px 1px #ffffff9e;*/
}

.v-num-input p {
  padding: 0.20rem;
  margin: 0 5px;
  color: var(--text-color);
}

</style>
