<script setup>
import { computed } from "vue";
import { COLOR_OPTIONS } from "./../colors";

const color = defineModel({ type: String, default: null });

const colorOptions = COLOR_OPTIONS;

const sampleClass = computed(() => {
  const option = colorOptions.filter((o) => {
    return o.value === color.value;
  })[0];

  return `bg-${option.label.toLowerCase()}`;
});
</script>

<template>
  <div class="color-picker">
    <label for="color-select">Select a color</label>
    <select id="color-select" v-model="color">
      <option
        v-for="(opt, i) in colorOptions"
        :key="`${opt.label}-${i}`"
        :value="opt.value"
      >
        {{ opt.label }}
      </option>
    </select>
    <div class="color-cell" :class="sampleClass" />
  </div>
</template>

<style>
.color-picker {
  display: flex;
  gap: 10px;

  select {
    width: auto;
  }

  .color-sample {
    width: 10px;
    height: 15px;
  }
}
</style>