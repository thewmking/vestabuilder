<script setup>
import { ref } from "vue";
import CellGrid from "./components/CellGrid.vue";
import ColorPicker from "./components/ColorPicker.vue";

const color = ref('#2c2c2c');
</script>

<template>
  <div id="main">
    <h1>vestabuilder</h1>
    <div class="components-wrapper">
      <ColorPicker v-model="color"/>
      <CellGrid :color="color" />
    </div>
  </div>
</template>

<style>
html {
  background: #2C2C2C;
}
body {
  font: 'Courier';
  font-family: monospace;

  .bg-red {
    background: #da291c;
  }
  .bg-orange {
    background: #f67502;
  }
  .bg-yellow {
    background: #f9b81d;
  }
  .bg-blue {
    background: #2e84d5;
  }
  .bg-green {
    background: #26853f;
  }
  .bg-purple {
    background: #702f8a;
  }
  .bg-white {
    background: #ffffff;
  }
  .bg-black {
    background: #2c2c2c;
  }

  .color-cell {
    width: 12px;
    height: 17px;
    border: 1px solid grey;
    cursor: pointer;
  }
}

#app {
  margin: 100px auto;

  #main {
    max-width: 450px;
    margin-left: auto;
    margin-right: auto;

    color: #E7F6F2;
  }
}
</style>
