<template>
  <main :class="['main', {selected: selected, hide: hide}]" @click="select">
      <div :class="['wall', 'horizontal', 'topwall', {base: orientation == 180}]">
        <div v-for="x in 20" class="brick"></div>
      </div>
      <div :style="topDoorStyle" class="door-style"></div>
      <div :class="['wall', 'horizontal', 'bottomwall', {base: orientation == 0}]">
        <div v-for="x in 20" class="brick"></div>
      </div>
      <div :style="bottomDoorStyle" class="door-style"></div>
      <div :class="['wall', 'vertical', 'leftwall', {base: orientation == 90}]">
        <div v-for="x in 20" class="brick"></div>
      </div>
      <div :style="leftDoorStyle" class="door-style"></div>
      <div :class="['wall', 'vertical', 'rightwall', {base: orientation == 270}]">
        <div v-for="x in 20" class="brick"></div>
      </div>
      <div :style="rightDoorStyle" class="door-style"></div>
      <div :style="brickStyle" class="bricks">
        <div :style="brickRowStyle" class="brick-row" v-for="x in brickRows"></div>
      </div>
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
      brickRows: {
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
      hide() {
        if (
          this.topWallDoor.size ||
          this.bottomWallDoor.size ||
          this.leftWallDoor.size ||
          this.rightWallDoor.size
        ) {
          return false;
        }
        return true
      },
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
            right: '0'
          }
        }
      },
      brickStyle() {
        if (this.orientation == 0) {
          return {
            top: '35px',
            left: '10px',
            flexDirection: 'column',
          }
        } else if (this.orientation == 180) {
          return {
            bottom: '35px',
            left: '10px',
            flexDirection: 'column',
          }
        } else if (this.orientation == 90) {
          return {
            right: '35px',
            top: '10px',
            flexDirection: 'row',
          }
        } else if (this.orientation == 270) {
          return {
            left: '35px',
            top: '10px',
            flexDirection: 'row',
          }
        }
      },
      brickRowStyle() {
        if (this.orientation == 0 || this.orientation == 180) {
          return {
          width: '179px',
          height: '11px',
          }
        }
         else if (this.orientation == 90 || this.orientation == 270) {
          return {
          width: '11px',
          height: '179px',
          }
        }
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
  position: relative;
  width: 200px;
  height: 200px;
  background-color: aqua;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #000;
  border: 3px solid #fff;
  font-size: 20px;
}
.selected {
  border: 3px solid #f00;
}

.hide {
  opacity: 0.5;
}
.wall {
  position: absolute;
  background-color: rgb(85, 85, 85);
  z-index: 1;
}

.base {
  background-color: lightgray;
  z-index: 0;
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

.door-style {
  background-color: aqua;
  position: absolute;
  z-index: 2;
}

.bricks {
  position: absolute;
  display: flex;
}
.brick-row {
  border: 1px solid #ccc;
  background-color:orangered;
}
</style>