<template>
  <main class="main">
    <div class="row" v-for="row in rows" :key="row.index">
      <div class="column" v-for="column in columns" :key="column.index">
        <Room :row="row" :column="column"/>
      </div>
    </div>
  </main>
</template>

<script>
  import Room from './components/Room.vue';

  export default {
    components: {
      Room
    },
    data() {
      return {
        rows: 3,
        columns: 4,
        layout: [],
      }
    },
    mounted() {
      this.update()
    },
    methods: {
      update() {
        for (let y = 1; y <= this.rows; y++) {
          for (let x = 1; x <= this.columns; x++) {
            this.layout.push({
              index: [x, y],
            })
          }
        }
      }
    }
  }
</script>

<style scoped>
.row {
  display: flex;
  flex-direction: row;
}

.column {
  display: flex;
  flex-direction: column;
}
</style>
