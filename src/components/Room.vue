<template>
  <main class="main">
    <div class="inner">
      <span>{{ row }}</span>
      <span>{{ column }}</span>
    </div>
  </main>
</template>

<script>
  export default {
    name: 'room',
    props: {
      row: {
        type: Number,
        required: true,
      },
      column: {
        type: Number,
        required: true,
      }
    },
    data() {
      return {
        //
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