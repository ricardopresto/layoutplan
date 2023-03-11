<template>
  <div class="panel">
    <input type="text" name="orientation" v-model="orientation">
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
      }
    },
    watch: {
      selectedRoom() {
        this.orientation = this.selectedRoom.orientation;
      }

    },
    methods: {
      update() {
        this.$emit('update-room', Number(this.orientation));
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