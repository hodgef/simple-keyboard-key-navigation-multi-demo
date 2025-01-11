<template>
  <div id="app">
    <!-- Input 1 -->
    <input
      id="input1"
      :value="input1"
      class="input"
      @input="onInputChange"
      @focus="onInputFocus"
      @blur="onInputBlur"
      placeholder="input1"
    />
    <SimpleKeyboard
      @onChange="onChange"
      @onKeyPress="onKeyPress"
      :input="input1"
      :inputFocused="input1Focused"
      inputName="input1"
      keyboardClass="input1SimpleKeyboard"
      theme="simple-keyboard"
    />

    <!-- Input 2 -->
    <input
      id="input2"
      :value="input2"
      class="input"
      @input="onInputChange"
      @focus="onInputFocus"
      @blur="onInputBlur"
      placeholder="input2"
    />
    <SimpleKeyboard
      @onChange="onChange"
      @onKeyPress="onKeyPress"
      :input="input2"
      :inputFocused="input2Focused"
      inputName="input2"
      keyboardClass="input2SimpleKeyboard"
      theme="simple-keyboard"
    />
  </div>
</template>

<script>
import SimpleKeyboard from "./SimpleKeyboard";
import "./App.css";

export default {
  name: "App",
  components: {
    SimpleKeyboard,
  },
  data: () => ({
    input1: "",
    input2: "",
    input1Focused: false,
    input2Focused: false,
    currentInput: ""
  }),
  mounted() {
    console.log("Listeners attached");
    document.addEventListener(
      "keydown",
      e => {
        const currentKeyboard = this.getCurrentKeyboard();
        console.log("eventListener", currentKeyboard, e.key);
        if(this.currentInput && currentKeyboard){
          if (e.key === "ArrowUp") currentKeyboard.modules.keyNavigation.up();
          else if (e.key === "ArrowDown") currentKeyboard.modules.keyNavigation.down();
          else if (e.key === "ArrowLeft") currentKeyboard.modules.keyNavigation.left();
          else if (e.key === "ArrowRight") currentKeyboard.modules.keyNavigation.right();
          else if (e.key === "Enter") currentKeyboard.modules.keyNavigation.press();
        }
      },
      false
    );
  },
  methods: {
    onChange(input, inputName) {
      this[inputName] = input;
      console.log("onChange", inputName);
    },
    onKeyPress(button) {
      console.log("button", button);
    },
    onInputChange(input) {
      console.log("onInputChange", input.target.id)
      this[input.target.id] = input.target.value;
    },
    onInputFocus(input) {
      console.log("onInputFocus", input.target.id)
      this[input.target.id + "Focused"] = true;
      this.currentInput = input.target.id;
    },
    onInputBlur(input) {
      console.log("onInputBlur", input.target.id)
      this[input.target.id + "Focused"] = false;
    },
    getCurrentKeyboard(){
      if(this.currentInput && window.SimpleKeyboardInstances){
        const keyboardId = this.currentInput + "SimpleKeyboard"
        return window.SimpleKeyboardInstances[keyboardId.toLowerCase()];
      }
    }
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
