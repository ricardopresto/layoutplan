<template>
  <main class="main">
    <div class="grid">
      <div v-for="room in json.roomData" >
          <Room 
            :index="room.index"
            :orientation="room.orientation"
            :brickColor="room.brickColor"
            :topWallDoor="room.topWallDoor"
            :bottomWallDoor="room.bottomWallDoor"
            :leftWallDoor="room.leftWallDoor"
            :rightWallDoor="room.rightWallDoor"
            @select="selectRoom"
          />
      </div>
    </div>
    <ControlPanel
      :selectedRoom="selectedRoom"
      @synchronise="synchronise"
      @generate="generateTemplate"
      @update-room="update"
    />
  </main>
</template>

<script>
  import Room from './components/Room.vue';
  import ControlPanel from './components/ControlPanel.vue';

  export default {
    components: {
      Room,
      ControlPanel
    },
    data() {
      return {
        json: {},
        rows: null,
        columns: null,
        selectedRoom: null,
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

      selectRoom(data) {
        this.selectedRoom = this.json.roomData.find(room => {
          return room.index == data
        });
        console.log(this.selectedRoom.index + ' selected')
      },

      update(data) {
        console.log(data)
        this.selectedRoom.orientation = data;
      }
    }
  }
</script>

<style scoped>
.main {
  display: flex;
  flex-direction: row;
}

.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-column-gap: 0px;
  grid-row-gap: 0px; 
}
</style>
