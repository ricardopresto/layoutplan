<template>
  <main :class="['main', {selected: selected}]" @click="select">
      <div :class="['wall', 'horizontal', 'topwall', {base: orientation == 180}]">
        <div v-for="x in 20" class="brick"></div>
      </div>
      <div :style="topDoorStyle"></div>
      <div :class="['wall', 'horizontal', 'bottomwall', {base: orientation == 0}]">
        <div v-for="x in 20" class="brick"></div>
      </div>
      <div :style="bottomDoorStyle"></div>
      <div :class="['wall', 'vertical', 'leftwall', {base: orientation == 90}]">
        <div v-for="x in 20" class="brick"></div>
      </div>
      <div :style="leftDoorStyle"></div>
      <div :class="['wall', 'vertical', 'rightwall', {base: orientation == 270}]">
        <div v-for="x in 20" class="brick"></div>
      </div>
      <div :style="rightDoorStyle"></div>
      <span>{{ index }}</span><br>
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
    computed: {
      topDoor() {
        return [
          this.topWallDoor.size?.[0],
          this.topWallDoor.size?.[1],
        ]  
      },
      topDoorStyle() {
        if (this.topDoor[0]) {
          return {
            left: `${(this.topDoor[0] - 1) * 10}px`,
            width: `${(this.topDoor[1] - this.topDoor[0] + 1) * 10}px`,
            height: '10px',
            backgroundColor: 'aqua',
            position: 'absolute',
            top: '0'
          }
        }
      },
      bottomDoor() {
        return [
          this.bottomWallDoor.size?.[0],
          this.bottomWallDoor.size?.[1],
        ]  
      },
      bottomDoorStyle() {
        if (this.bottomDoor[0]) {
          return {
            left: `${(this.bottomDoor[0] - 1) * 10}px`,
            width: `${(this.bottomDoor[1] - this.bottomDoor[0] + 1) * 10}px`,
            height: '10px',
            backgroundColor: 'aqua',
            position: 'absolute',
            bottom: '0'
          }
        }
      },
      leftDoor() {
        return [
          this.leftWallDoor.size?.[0],
          this.leftWallDoor.size?.[1],
        ]  
      },
      leftDoorStyle() {
        if (this.leftDoor[0]) {
          return {
            top: `${(this.leftDoor[0] - 1) * 10}px`,
            height: `${(this.leftDoor[1] - this.leftDoor[0] + 1) * 10}px`,
            width: '10px',
            backgroundColor: 'aqua',
            position: 'absolute',
            left: '0'
          }
        }
      },
      rightDoor() {
        return [
          this.rightWallDoor.size?.[0],
          this.rightWallDoor.size?.[1],
        ]  
      },
      rightDoorStyle() {
        if (this.rightDoor[0]) {
          return {
            top: `${(this.rightDoor[0] - 1) * 10}px`,
            height: `${(this.rightDoor[1] - this.rightDoor[0] + 1) * 10}px`,
            width: '10px',
            backgroundColor: 'aqua',
            position: 'absolute',
            right: '0'
          }
        }
      },
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
  position: relative;
  width: 200px;
  height: 200px;
  background-color: aqua;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 5px;
  color: #000;
  border: 2px solid #fff;
  font-size: 20px;
}
.selected {
  border: 2px solid #f00;
}

.wall {
  position: absolute;
  background-color: gray;
}

.base {
  background-color: lightgray;
}
.wall.horizontal {
  width: 200px;
  height: 10px;
  display: flex;
  flex-direction: row;
}
.wall.vertical {
  width: 10px;
  height: 200px;
  display: flex;
  flex-direction: column;
}

.topwall {
  top: 0;
}

.bottomwall {
  bottom: 0;
}

.leftwall {
  left: 0;
}

.rightwall {
  right: 0;
}

.brick {
  width: 8px;
  height: 8px;
  border: 1px solid #ccc;
}
</style>