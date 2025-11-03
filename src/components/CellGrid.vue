<script setup>
import { ref } from "vue";
import { COLOR_BLACK, COLOR_WHITE, hexCodeMap, hexLabelMap } from "./../colors";

const props = defineProps({
  color: { type: String, required: true }
})

const rowCount = 6;
const colCount = 22;

const defaultCells = () => {
  const colArray = `${COLOR_BLACK},`.repeat(colCount).split(',');
  const filtered = colArray.filter((i) => i != '');

  const grid = [];

  for (let index = 0; index < rowCount; index++) {
    grid.push(structuredClone(filtered));
  }

  return grid
};

const cells = ref(defaultCells());
const filename = ref('vesta-array');

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


const exportGrid = () => {
  const convertedCells = [];

  cells.value.forEach((row) => {
    const mappedRow = row.map((c) => {
      return hexCodeMap[c];
    });
    convertedCells.push(mappedRow);
  });

  downloadFile(JSON.stringify(convertedCells), `${filename.value}.txt`, 'txt');
}

const downloadFile = (content, fileName, contentType) => {
  const a = document.createElement("a");
  const file = new Blob([content], {type: contentType});
  a.href = URL.createObjectURL(file);
  a.download = fileName;
  a.click();
};
</script>

<template>
  <div class="cell-grid-wrapper">
    <p>Click cells to color!</p>
    <div class="cell-grid">
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
    <div class="export-section">
      <div class="form-group">
        <label for="filename">Provide a file name</label>
        <input id="filename" type="text" v-model="filename" />
      </div>
      <button @click="exportGrid">Download json txt</button>
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

.export-section {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  row-gap: 20px;
  align-items: flex-start;
}

.form-group {
  display: flex;
  column-gap: 20px;
  align-items: center;
}
</style>