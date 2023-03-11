<template>
  <div class="panel">
    <label for="orientation">Orientation</label>
    <input type="number" name="orientation" v-model="orientation">
    <label for="topWallDoor">Top wall door</label>
    <div class="wallgroup">
      <input type="number" name="topWallDoor" v-model="topWallDoorStart">
      <input type="number" name="topWallDoor" v-model="topWallDoorEnd">
    </div>
    <button @click="update">Update room</button>
    <button @click="$emit('synchronise')">Synchronise all</button>
    <button :disabled="disableGenerate" @click="$emit('generate')">Generate template</button>
  </div>
</template>

<script>
  export default {
    name: 'control-panel',
    emits: ['generate', 'synchronise', 'update-room'],
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
        topWallDoor: {},
        bottomWallDoor: {},
        leftWallDoor: {},
        rightWallDoor: {},
      }
    },
    watch: {
      selectedRoom() {
        this.orientation = this.selectedRoom.orientation;
        this.topWallDoorStart = this.selectedRoom.topWallDoor.size?.[0] ?? null;
        this.topWallDoorEnd = this.selectedRoom.topWallDoor.size?.[1] ?? 0;
        this.bottomWallDoor = this.selectedRoom.bottomWallDoor;
        this.leftWallDoor = this.selectedRoom.leftWallDoor;
        this.rightWallDoor = this.selectedRoom.rightWallDoor;
      }
    },
    methods: {
      update() {
        this.$emit('update-room', {
          orientation: this.orientation,
          topWallDoor: this.getTopDoor() 
        })
      },

      getTopDoor() {
        if (this.topWallDoorStart > 0 && this.topWallDoorEnd > 0 ) {
          return {
            size: [
            this.topWallDoorStart,
            this.topWallDoorEnd,
            ],
            toRoom: this.getRoomToTop(this.selectedRoom.index),
          };
        }

        return {};
      },

      getRoomToTop(index) {
        const row = Number(index.substring(0, 1));
        return `${row - 1}${index.substring(1)}`;
      }
    }
  }
</script>

<style>
.panel {
  display: flex;
  flex-direction: column;
  width: 200px;
 }

 button {
  height: 30px;
  margin: 10px;
 }
</style>