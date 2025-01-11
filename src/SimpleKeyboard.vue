<template>
  <div :class="keyboardClass"></div>
</template>

<script>
import Keyboard from "simple-keyboard";
import "simple-keyboard/build/css/index.css";

import keyNavigation from "simple-keyboard-key-navigation";

export default {
  name: "SimpleKeyboard",
  props: {
    keyboardClass: {
      default: "simple-keyboard",
      type: String
    },
    theme: {
      type: String
    },
    input: {
      type: String
    },
    inputName: {
      type: String
    },
    inputFocused: {
      type: Boolean
    }
  },
  data: () => ({
    keyboard: null,
    layoutName: "default"
  }),
  mounted() {
    this.initKeyboard();
  },
  methods: {
    onChange(input) {
      this.$emit("onChange", input, this.inputName);
    },
    onKeyPress(button) {
      this.$emit("onKeyPress", button);

      /**
       * If you want to handle the shift and caps lock buttons
       */
      if (button === "{shift}" || button === "{lock}") this.handleShift();
    },
    handleShift() {
      let currentLayout = this.keyboard.options.layoutName;
      let shiftToggle = currentLayout === "default" ? "shift" : "default";

      this.layoutName = shiftToggle;
      this.keyboard.setOptions({
        layoutName: shiftToggle
      });
    },
    initKeyboard(options = {}){
      this.keyboard = new Keyboard(this.keyboardClass, {
        theme: this.theme + " hg-theme-default",
        onChange: this.onChange,
        onKeyPress: this.onKeyPress,
        disableButtonHold: true,
        beforeRender: (keyboard) => {
          keyboard.options.layoutName = this.layoutName;
        },
        onRender: (keyboard) => {
          keyboard.setInput(this.input);
        },
        ...options
      });
    }
  },
  watch: {
    input(input) {
      this.keyboard.setInput(input);
    },
    inputFocused(status) {
      console.log("inputFocused", this.inputName, status);

      this.keyboard.destroy();

      if(status) {
        this.initKeyboard({
          modules: [keyNavigation],
          enableKeyNavigation: true
        })
      } else {
        this.initKeyboard();
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
