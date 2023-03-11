<template>
  <main :class="['main', {selected: selected}]" @click="select">
    <div class="inner">
      <span>{{ index }}</span>
    </div>
  </main>
</template>

<script>
  export default {
    name: 'room',
    props: {
      index: {
        type: String,
      },
      orientation: {
        type: Number
      },
      brickColor: {
        type: String
      },
      topWallDoor: {
        type: Object
      },
      bottomWallDoor: {
        type: Object
      },
      leftWallDoor: {
        type: Object
      },
      rightWallDoor: {
        type: Object
      },
      selected: {
        type: Boolean
      }
    },
    emits: ['select'],
    data() {
      return {
        roomIndex: this.index,
      }
    },
    methods: {
      select() {
        this.$emit('select', this.roomIndex);
      }
    }
  }
</script>

<style scoped>
.main {
  width: 200px;
  height: 200px;
  background-color: aqua;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 5px;
  color: #fff;
  border: 2px solid #fff;
}

.selected {
  border: 2px solid #f00;
}
.inner {
  width: 100px;
  height: 100px;
  background-color: blue;
}
</style>