<template>
  <div id="app">
    <button v-on:click="startGame">Start Game</button>
    <table class="minesweeper">
      <tr
        v-for="(row, rowIndex) in tiles"
        v-bind:key="rowIndex"
      >
        <VTile
          v-for="(tile, columnIndex) in row"
          :key="columnIndex"
          :data="tile"
          @leftClick="openTile"
          @rightClick="setFlag"
        >
        </VTile>
      </tr>
    </table>
  </div>
</template>

<script>
import VTile from './components/VTile';

export default {
  name: 'App',
  components: {
    VTile,
  },
  data: () => {
    return {
      rows: 10,
      columns: 19,
      tiles: [],
      isStart: false,
      isSuccess: false,
      isFailure: false,
      neighbour: ['mine-neighbor-1', 'mine-neighbor-2', 'mine-neighbor-3', 'mine-neighbor-4', 'mine-neighbor-5', 'mine-neighbor-6',
        'mine-neighbor-7', 'mine-neighbor-8', 'mine-neighbor-9'],
    };
  },
  methods: {
    /**
    * flags a tile
    * @function
    * @param {Object} tile
    * @return {undefined}
    */
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
    /**
    * opens a tile
    * @function
    * @param {Object} tile
    * @return {undefined}
    */
    openTile: function(tile) {
      const numNeighborMines = this.countNeighboringMines(tile.row, tile.column);
      this.changeTileClass(tile.row, tile.column, numNeighborMines);
    },
    /**
    * flags a tile
    * @function
    * @param {Object} tile
    * @return {undefined}
    */
    setFlag: function(tile) {
      if (this.isFailure || tile.class === 'opened') {
        return;
      }
      if (tile.class === 'flagged') {
        tile.class = 'unopened';
      } else {
        tile.class = 'flagged';
      }
    },
    /**	
     * opens all tiles	
     * @function	
     * @return {undefined}	
     */	
    showAllTiles: function() {	
    },	
    /**	
     * returns the number of mines within an array of tiles	
     * @function	
     * @param {Array.<Object>} neighbors - an array of tile objects	
     * @return {number} - number of mines in neighbors array	
     */	
    countNeighboringMines: function(tile) {	
      let countNeighborMine = 0;
      const selectNeighboringTiles = [
        { row:  1, col:  0 },
        { row:  1, col:  1 },
        { row:  0, col:  1 },
        { row: -1, col:  1 },
        { row: -1, col:  0 },
        { row: -1, col: -1 },
        { row:  0, col: -1 },
        { row:  1, col: -1 },
      ];
      selectNeighboringTiles.forEach(neighborTile => {
        const neighborRow = tile.row + neighborTile.row;
        const neighborCol = tile.col + neighborTile.col;
        if (!this.isValid(neighborRow, neighborCol)) {
          return;
        }
        const neigbor = this.tiles[neighborRow][neighborCol];
        if (neigbor.isMined) {
          countNeighborMine += 1;
        }
      });
      return countNeighborMine;	
    },
    /**
     * change ClassName Based on MineCount
     * @function
     * @param {number} row
     * @param {number} col
     * @param {number} numNeighborMines
     * @return {undefined}
     */
    changeTileClass: function(row, col, numNeighborMines) {
      if (numNeighborMines > 9) {
        return null;
      }
      this.tiles[row][col].state = `mine-neighbor-${numNeighborMines}`;
      if (numNeighborMines == 0) {
        return this.tiles[row][col].state = 'opened';
      }
      if (this.tiles[row][col].mined) {
        return this.tiles[row][col].state = 'mine';
      }
    },	
    /**	
     * valid tile
     * @function	
     * @param {Object} tile	
     * @return {boolean}	
     */	
    isValid: function(row, column) {
      return !(this.tiles[row] && this.tiles[row][column]);
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
