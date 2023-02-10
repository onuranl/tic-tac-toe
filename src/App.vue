<script>
export default {
  data: () => ({
    gameState: true,
    items: [{ name: 'X', state: true }, { name: 'O', state: false }],
    cells: Array.from({ length: 9 }, (_, i) => { return { key: null, id: i } })
  }),
  watch: {
    cells: {
      deep: true,
      handler(val) {
        const states = {
          'horizontal': [[0, 1, 2], [3, 4, 5], [6, 7, 8]],
          'vertical': [[0, 3, 6], [1, 4, 7], [2, 5, 8]],
          'cross': [[0, 4, 8], [2, 4, 6]]
        }

        Object.values(states).map(state => {
          state.map(item => {
            if (!this.haveAnyNull(item)) {
              if (this.allEqual(item.map(el => val[el].key))) {
                this.gameState = false
              }
            }
          })
        })
      }
    },
    gameState(val) {
      if (!val) location.reload()
    }
  },
  computed: {
    currentItem() {
      return this.items.find(item => item.state).name
    },
  },
  methods: {
    fillCell(id) {
      this.cells[id].key = this.currentItem

      this.changeItemsState()
    },
    changeItemsState() {
      this.items = this.items.map(item => {
        item.state = !item.state

        return item
      })
    },
    haveAnyNull(item) {
      return item.some(el => this.cells[el].key === null)
    },
    allEqual(item) {
      return item.every((val, i, arr) => val === arr[0])
    }
  }
}
</script>>

<template>
  <div>
    <div class="cells">
      <div v-for="cell in cells" :class="{ cell, disable: cell.key }" :id="cell.id" v-html="cell.key"
        @click="fillCell(cell.id)" />
    </div>
  </div>
</template>

<style scoped>
.cells {
  background-color: white;
  display: grid;
  column-gap: 10px;
  row-gap: 10px;
  grid-template-columns: auto auto auto;
}

.cell {
  border: 1px solid rgba(0, 0, 0, 0.8);
  width: 150px;
  height: 150px;
  color: black;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 30px;
  cursor: pointer;
}

.disable {
  pointer-events: none;
}
</style>