<template>
  <div id="app">
    <button v-on:click="startGame">Start Game</button>
    <table class="minesweeper">
      <tr
        v-for="(row, rowIndex) in tiles"
        v-bind:key="rowIndex"
      >
        <Square
          v-for="(tile, columnIndex) in row"
          :key="columnIndex"
          :data="tile"
        >
        </Square>
      </tr>
    </table>
  </div>
</template>

<script>
import Square from './components/Tile';

export default {
  name: 'App',
  components: {
    Square,
  },
  data: () => {
    return {
      rows: 10,
      columns: 19,
      tiles: [],
    };
  },
  methods: {
    startGame: function() {
      this.tiles = [];
      for (let rowIndex = 0; rowIndex < this.rows; rowIndex++) {
        let row = [];
        for (let columnIndex = 0; columnIndex < this.columns; columnIndex++) {
          let tile = {
            row: rowIndex,
            column: columnIndex,
            mined: Math.random() * 6 > 5,
            class: 'unopened',
          };
          row.push(tile);
        }
        this.tiles.push(row);
      }
    },
  },
};
</script>

<style>
body {
  font-family: Helvetica, sans-serif;
  background: #333;
  color: #fff;
}

button {
  margin: 20px auto;
  display: block;
}

table.minesweeper {
  margin: auto;
  border: 1px solid #999;
  border-collapse: collapse;
  background-color: #ddd;
}

table.minesweeper td {
  width: 24px;
  height: 24px;
  background-size: cover;
}
</style>
