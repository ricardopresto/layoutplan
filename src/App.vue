<template>
  <main class="main">
    <div class="grid">
      <div class="row" v-for="row in rows" :key="row.index">
        <div class="column" v-for="column in columns" :key="column.index">
          <Room :row="row" :column="column" @update="updateRoom"/>
        </div>
      </div>
      <div class="panel">
        <button @click="synchronise">Synchronise</button>
        <button :disabled="disableGenerate" @click="generateTemplate">Generate template</button>
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
        json: {},
        rows: null,
        columns: null,
        disableGenerate: true,
      }
    },
    mounted() {
      this.readFile();
    },
    methods: {
      generateTemplate() {
        const setRows = 3;
        const setColumns = 3;
        const template = {};
        const roomData = [];
        template.meta = {
          rows: setRows,
          columns: setColumns,
        };

        for (let row = 1; row <= setRows; row++) {
          for (let column = 1; column <= setColumns; column++) {
            roomData.push({
              'index': `${row}-${column}`,
              'orientation': 0,
              'brickColor': '',
              'hitBricks': [],
              'topWallDoor': {},
              'bottomWallDoor': {},
              'leftWallDoor': {},
              'rightWallDoor': {}
            })
          }
        }
        template.roomData = roomData;
        this.writeFile(template);
        this.readFile();
      },

      synchronise() {
        this.writeFile(this.json)
        this.readFile()
      },

      writeFile(data) {
        fetch('http://localhost:3000/write', {
          method: 'POST',
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(data),
        });
      },

      async readFile() {
        await fetch('http://localhost:3000/read', {
          method: 'GET'
        })
        .then(response => response.json())
        .then(data => {
          const json = JSON.parse(data);
          this.json = json;
          this.rows = json.meta.rows;
          this.columns = json.meta.columns;
        });
      },

      updateRoom(data) {
        const selectedRoom = this.json.roomData.find(room => {
          return room.index == data
        });
        selectedRoom.orientation = 0;
        console.log('room ' + data + ' updated');
        this.synchronise();
      }
    }
  }
</script>

<style scoped>
.main {
  display: flex;
  flex-direction: row;
}

.row {
  display: flex;
  flex-direction: row;
}

.column {
  display: flex;
  flex-direction: column;
}

.panel
 {
  display: flex;
  flex-direction: column;
  width: 200px;
 }

 button {
  height: 30px;
  margin: 10px;
 }
</style>
