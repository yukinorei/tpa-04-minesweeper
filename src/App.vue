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
  data: function() {
    return {
      rows: 10,
      columns: 19,
      tiles: [],
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
            isMined: Math.random() * 6 > 5,
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
      if (tile.isMined) {
        this.changeTileClass(tile, 'mine');
        this.showAllTiles();
      } else {
        const neighborMines = this.countNeighboringMines(tile);
        const numNeighborMines = neighborMines.filter(neighborTile => neighborTile.isMined).length;
        if (numNeighborMines > 0) {
          this.changeTileClass(tile, `mine-neighbor-${numNeighborMines}`);
        } else {
          this.changeTileClass(tile, 'opened');
          neighborMines.forEach(neighborTile => {
            if (!this.isTileOpened(neighborTile)) this.openTile(neighborTile);
          });
        }
      }
    },
    /**
    * flags a tile
    * @function
    * @param {Object} tile
    * @return {undefined}
    */
    setFlag: function(tile) {
      if (this.isTileOpened(tile)) return;
      if (tile.class === 'flagged') {
        this.changeTileClass(tile, 'unopened');
      } else {
        this.changeTileClass(tile, 'flagged');
      }
    },
    /**	
     * opens all tiles	
     * @function	
     * @return {undefined}	
     */	
    showAllTiles: function() {	
      this.tiles.forEach(tilesRow => {
        tilesRow.forEach(tile => {
          if (this.isTileOpened(tile)) return;
          if (tile.isMined) {
            tile.class = 'mine';
          } else {
            const neighborTiles = this.countNeighboringMines(tile);
            const numNeighborMines = neighborTiles.filter(neighborTile => neighborTile.isMined).length;
            if (numNeighborMines > 0) {
              tile.class = `mine-neighbor-${numNeighborMines}`;
            } else {
              tile.class = 'opened';
            }
          }
        });
      });
    },	
    /**	
     * returns the number of mines within an array of tiles	
     * @function	
     * @param {Array.<Object>} neighbors - an array of tile objects	
     * @return {number} - number of mines in neighbors array	
     */	
    countNeighboringMines: function(tile) {	
      let neighborTiles = [];
      const selectNeighboringTiles = [
        { row:  1, column:  0 },
        { row:  1, column:  1 },
        { row:  0, column:  1 },
        { row: -1, column:  1 },
        { row: -1, column:  0 },
        { row: -1, column: -1 },
        { row:  0, column: -1 },
        { row:  1, column: -1 },
      ];
      selectNeighboringTiles.forEach(neighborTile => {
        const neighborRow = tile.row + neighborTile.row;
        const neighborCol = tile.column + neighborTile.column;
        if (!this.isPosition(neighborRow, neighborCol)) {
          return;
        }
        const neighbor = this.tiles[neighborRow][neighborCol];
        neighborTiles.push(neighbor);

      });
      return neighborTiles;	
    },
    /**
     * change ClassName Based on MineCount
     * @function
     * @param {number} row
     * @param {number} col
     * @param {number} numNeighborMines
     * @return {undefined}
     */
    changeTileClass: function(tile, newTileClass) {
      this.tiles[tile.row][tile.column].class = newTileClass;
    },	
    isPosition: function(row, column) {
      return (0 <= row && row <= this.rows - 1) && (0 <= column && column <= this.columns - 1);
    },
    isTileOpened: function(tile) {
      return tile.class !== 'unopened' && tile.class !== 'flagged';
    }	
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
