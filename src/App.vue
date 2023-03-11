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
            :selected="selectedRoomIndex == room.index"
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

    computed: {
      selectedRoomIndex() {
        return this.selectedRoom?.index;
      },
    },

    watch: {
      json() {
        this.selectedRoom = this.json.roomData.find(room => {
          return room.index == this.selectedRoomIndex;
        });
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
            const index = `${row}-${column}`;

            roomData.push({
              'index': index,
              'orientation': 0,
              'brickColor': '',
              'hitBricks': [],
              'topWallDoor': {
                'size': null,
                'toRoom': this.getRoomToTop(index),
              },
              'bottomWallDoor': {
                'size': null,
                'toRoom': this.getRoomToBottom(index),
              },
              'leftWallDoor': {
                'size': null,
                'toRoom': this.getRoomToLeft(index),
              },
              'rightWallDoor': {
                'size': null,
                'toRoom': this.getRoomToRight(index),
              }
            })
          }
        }
        template.roomData = roomData;
        this.writeFile(template);
        this.readFile();
      },

      synchronise() {
        this.writeFile(this.json);
        this.readFile();
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
          this.json = JSON.parse(data);
          this.rows = this.json.meta.rows;
          this.columns = this.json.meta.columns;
        });
      },

      selectRoom(data) {
        this.selectedRoom = this.json.roomData.find(room => {
          return room.index == data;
        });
      },

      update(data) {
        console.log(data)
        this.selectedRoom.orientation = data.orientation;
        this.setTopWall(data.topWallDoorSize);
        this.setBottomWall(data.bottomWallDoorSize);
        this.setLeftWall(data.leftWallDoorSize);
        this.setRightWall(data.rightWallDoorSize);
      },

      setTopWall(data) {
        const roomToTop = this.json.roomData.find(room => {
          return room.index == this.selectedRoom.topWallDoor.toRoom;
        });
        if (roomToTop) {
          this.selectedRoom.topWallDoor.size = data;
          roomToTop.bottomWallDoor.size = data;
        }
      },

      setBottomWall(data) {
        const roomToBottom = this.json.roomData.find(room => {
          return room.index == this.selectedRoom.bottomWallDoor.toRoom;
        });
        if (roomToBottom) {
          this.selectedRoom.bottomWallDoor.size = data;
          roomToBottom.topWallDoor.size = data;
        }
      },

      setLeftWall(data) {
        const roomToLeft = this.json.roomData.find(room => {
          return room.index == this.selectedRoom.leftWallDoor.toRoom;
        });
        if (roomToLeft) {
          this.selectedRoom.leftWallDoor.size = data;
          roomToLeft.rightWallDoor.size = data;
        }
      },

      setRightWall(data) {
        const roomToRight = this.json.roomData.find(room => {
          return room.index == this.selectedRoom.rightWallDoor.toRoom;
        });
        if (roomToRight) {
          this.selectedRoom.rightWallDoor.size = data;
          roomToRight.leftWallDoor.size = data;
        }
      },

      getRoomToTop(index) {
        const row = Number(index.substring(0, 1));
        return `${row - 1}${index.substring(1)}`;
      },

      getRoomToBottom(index) {
        const row = Number(index.substring(0, 1));
        return `${row + 1}${index.substring(1)}`;
      },

      getRoomToLeft(index) {
        const column = Number(index.substring(2));
        return `${index.substring(0, 2)}${column - 1}`;
      },

      getRoomToRight(index) {
        const column = Number(index.substring(2));
        return `${index.substring(0, 2)}${column + 1}`;
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
