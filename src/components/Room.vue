<template>
  <main class="main">
    <div class="inner" @click="select">
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
}

.inner {
  width: 100px;
  height: 100px;
  background-color: blue;
}
</style>