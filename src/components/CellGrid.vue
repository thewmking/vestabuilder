<script setup>
import { ref } from "vue";
import { COLOR_WHITE, hexLabelMap } from "./../colors";

const props = defineProps({
  color: { type: String, required: true }
})

const rowCount = 6;
const colCount = 22;

const defaultCells = () => {
  const colArray = `${COLOR_WHITE},`.repeat(colCount).split(',');
  const filtered = colArray.filter((i) => i != '');

  const grid = [];

  for (let index = 0; index < rowCount; index++) {
    grid.push(structuredClone(filtered));
  }

  return grid
};

const cells = ref(defaultCells());

const cellCoords = (row, col) => {
  return {
    rowNum: row - 1,
    colNum: col - 1,
  }
};

const cellColorClass = (row, col) => {
  if (!cells.value.length > 0) return '';

  const { rowNum, colNum } = cellCoords(row, col);

  const color = cells.value[rowNum][colNum];

  const label = hexLabelMap[color];

  return `bg-${label}`;
}

const selectCell = (row, col) => {
  const { rowNum, colNum } = cellCoords(row, col);

  cells.value[rowNum][colNum] = props.color;
};
</script>

<template>
  <div class="cell-grid" v-if="rowCount && colCount">
    <div class="grid-row" v-for="row in rowCount" :key="`row-${row}`">
      <div
          v-for="col in colCount"
          :key="`col-${col}`"
          :class="cellColorClass(row, col)"
          class="color-cell"
          @click="selectCell(row, col)"
      ></div>
    </div>
  </div>
</template>

<style>
.cell-grid {
  margin-top: 20px;
  display: flex;
  gap: 5px;
  flex-direction: column;

  .grid-row {
    display: flex;
    gap: 5px;
  }
}
</style>