<template>
  <div class="panel">
    <label>Orientation</label>
    <input type="number" v-model="orientation" min="0" max="270" step="90">
    <label>Brick rows</label>
    <input type="number" v-model="brickRows" min="1" max="3">
    <label>Top wall door</label>
    <div class="wallgroup">
      <input type="number" v-model="topWallDoorStart" min="1" max="20">
      <input type="number" v-model="topWallDoorEnd" min="1" max="20">
    </div>
    <label>Bottom wall door</label>
    <div class="wallgroup">
      <input type="number" v-model="bottomWallDoorStart" min="1" max="20">
      <input type="number" v-model="bottomWallDoorEnd" min="1" max="20">
    </div>
    <label>Left wall door</label>
    <div class="wallgroup">
      <input type="number" v-model="leftWallDoorStart" min="1" max="20">
      <input type="number" v-model="leftWallDoorEnd" min="1" max="20">
    </div>
    <label>Right wall door</label>
    <div class="wallgroup">
      <input type="number" v-model="rightWallDoorStart" min="1" max="20">
      <input type="number" v-model="rightWallDoorEnd" min="1" max="20">
    </div>
    <button @click="update">Update room</button>
    <button @click="$emit('overwrite')">Overwrite file</button>
    <button :disabled="disableGenerate" @click="$emit('generate')">Generate template</button>
  </div>
</template>

<script>
  export default {
    name: 'control-panel',
    emits: ['generate', 'overwrite', 'update-room'],
    props: {
      selectedRoom: {
        type: Object,
        default: {}
      }
    },
    data() {
      return {
        disableGenerate: true,
        orientation: null,
        brickRows: null,
        topWallDoorStart: null,
        topWallDoorEnd: null,
        bottomWallDoorStart: null,
        bottomWallDoorEnd: null,
        leftWallDoorStart: null,
        leftWallDoorEnd: null,
        rightWallDoorStart: null,
        rightWallDoorEnd: null,
      }
    },
    watch: {
      selectedRoom() {
        this.orientation = this.selectedRoom.orientation;
        this.brickRows = this.selectedRoom.brickRows;
        this.topWallDoorStart = this.selectedRoom.topWallDoor?.size?.[0] ?? null;
        this.topWallDoorEnd = this.selectedRoom.topWallDoor?.size?.[1] ?? null;
        this.bottomWallDoorStart = this.selectedRoom.bottomWallDoor?.size?.[0] ?? null;
        this.bottomWallDoorEnd = this.selectedRoom.bottomWallDoor?.size?.[1] ?? null;
        this.leftWallDoorStart = this.selectedRoom.leftWallDoor?.size?.[0] ?? null;
        this.leftWallDoorEnd = this.selectedRoom.leftWallDoor?.size?.[1] ?? null;
        this.rightWallDoorStart = this.selectedRoom.rightWallDoor?.size?.[0] ?? null;
        this.rightWallDoorEnd = this.selectedRoom.rightWallDoor?.size?.[1] ?? null;
      }
    },
    methods: {
      update() {
        this.$emit('update-room', {
          orientation: this.orientation,
          brickRows: this.brickRows,
          topWallDoorSize: this.getTopDoor(),
          bottomWallDoorSize: this.getBottomDoor(),
          leftWallDoorSize: this.getLeftDoor(),
          rightWallDoorSize: this.getRightDoor(),
        })
      },

      getTopDoor() {
        if (this.topWallDoorStart > 0 && this.topWallDoorEnd > 0 ) {
          return [
            this.topWallDoorStart,
            this.topWallDoorEnd,
          ]
        }
        return null;
      },

      getBottomDoor() {
        if (this.bottomWallDoorStart > 0 && this.bottomWallDoorEnd > 0 ) {
          return [
            this.bottomWallDoorStart,
            this.bottomWallDoorEnd,
          ]
        }
        return null;
      },

      getLeftDoor() {
        if (this.leftWallDoorStart > 0 && this.leftWallDoorEnd > 0 ) {
          return [
            this.leftWallDoorStart,
            this.leftWallDoorEnd,
          ]
        }
        return null;
      },

      getRightDoor() {
        if (this.rightWallDoorStart > 0 && this.rightWallDoorEnd > 0 ) {
          return [
            this.rightWallDoorStart,
            this.rightWallDoorEnd,
          ]
        }
        return null;
      },
    }
  }
</script>

<style>
.panel {
  display: flex;
  flex-direction: column;
  width: 200px;
 }

 label {
  margin-top: 10px;
 }

 input {
  margin: 5px;
  width: 80px;
 }

 button {
  height: 30px;
  margin: 10px;
 }
</style>