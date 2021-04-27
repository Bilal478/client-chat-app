<template>
  <div id="app">
    <el-container>
      <el-aside class="control_area">
        <!--        <div class="settings">-->
        <!--          <settings></settings>-->
        <!--        </div>-->
        <div class="side_blocks">
          <button @click="$refs.modalName.openModal()" style="cursor: pointer;" class="roombtn">
            Add room
          </button>

          <modal ref="modalName">
            <template v-slot:header>
              <h1>Create Room</h1>
            </template>

            <template v-slot:body>
              <input
                type="text"
                id="roomname"
                placeholder="Add Room name here"
                class="roominput"
              />
            </template>

            <template v-slot:footer>
              <div>
                <button @click="$refs.modalName.closeModal()">Cancel</button>
                <button
                  v-on:click="addroom"
                  @click="$refs.modalName.closeModal()"
                >
                  Add Room
                </button>
              </div>
            </template>
          </modal>
          <Channel_list
            :list="roomdata.rooms"
            :active="roomdata.current_room"
          ></Channel_list>
          <div class="direct_messages">
            <user_list :users="roomdata.users"></user_list>
          </div>
        </div>
      </el-aside>
      <el-container>
        <el-header class="channel_header">
          <h1>{{ roomdata.current_room }} Channel</h1>
        </el-header>
        <el-main class="massege_container" style="width: 90%;" id="data">
          <chat_body :messages="user_messages"></chat_body>
        </el-main>
        <el-footer class="footer">
          <el-row type="flex">
            <input type="textarea" placeholder="Type here" id="input" class="chat-input" />
            <el-button
              type="primary"
              style="
                background-color: #d0a159;
                border-radius: 0;
                color: black;
                font-size: 1.1rem;
                width: 80px;
                border: none;
              "
              class="send-btn"
              v-on:click="send"
              ><i class="fas fa-chevron-right"></i></el-button
            >
          </el-row>
        </el-footer>
      </el-container>
    </el-container>
  </div>
</template>
<script>
import Channel_list from "./components/Channel_list.vue";
import User_list from "./components/User_list.vue";
import settings from "./components/Settings.vue";
import chat_body from "./components/chat_body.vue";
import Modal from "./components/modal";

import store from "./store/store";
import io from "socket.io-client";
//import socket from "socket.io-client";

export default {
  name: "app",
  components: {
    Channel_list,
    User_list,
    settings,
    chat_body,
    Modal,
  },
  data() {
    return {
      chat_block: "sdsd",
    };
  },
  computed: {
    roomdata() {
      return this.$store.state.room_data;
    },
    user_messages() {
      return this.$store.state.messages;
    },
  },
  methods: {
    send() {
      var elem = document.getElementById("chat");
      elem.scrollTop = elem.scrollHeight;
      if (document.getElementById("input").value.length > 0) {
        socket.emit("chat_message", document.getElementById("input").value);
        document.getElementById("input").value = "";
      }
    },

    updateroom(val) {
      this.$store.commit('update_room', val)
    },

    addroom() {
      let roomname = document.getElementById("roomname").value;
      if(roomname.length > 0)
      {
        var myHeaders = new Headers();
      myHeaders.append("Content-Type", "application/x-www-form-urlencoded");

      var urlencoded = new URLSearchParams();
      urlencoded.append("roomname", roomname);

      var requestOptions = {
        method: "POST",
        headers: myHeaders,
        body: urlencoded,
        redirect: "follow",
      };

      fetch("http://localhost:3000/addroom", requestOptions)
        .then((response) => response.text())
        .then((result) => {
            this.updateroom(roomname);
            console.log(result)
        })
        .catch((error) => console.log("error", error));
    }
      }
  },
  created() {
    socket.on("connect", () => {
      socket.emit(
        "adduser",
        prompt("enter the name", `user${Math.floor(Math.random() * 100 + 1)}`)
      );
      socket.on("chat_update", (data) => {
        this.$store.dispatch("common_message", data);
      });
      socket.on("site_data", (data) => {
        this.$store.dispatch("adding_roomdata", data);
      });
    });
  },
};
</script>
<style lang="scss">

#app {
  font-family: 'Aventa-SemiBold', Helvetica, Arial, Sans-Serif;
  height: 100vh;
  color: #d2d2d2 !important;
  background-color: black;
}

/* width */
::-webkit-scrollbar {
  width: 10px;
}

/* Track */
::-webkit-scrollbar-track {
  background: black;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: #1a1919;
  border-radius: 10px;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: #242424;
}

// .channel_header {
//     padding: 10px;
//     background: #409EFF;
//     height: auto !important;
//     color: #fff;
//     display: flex;
//     align-items: center;
//     display: table-cell;

//     h1 {
//         padding: 10px 0px;
//     }

//     * {
//         padding: 0px;
//         margin: 0px;
//     }
// }

.channel_header {
  background-color: black;
  font-size: 1.3rem;
}

body {
  margin: 0px;
  height: 100vh;
  overflow: hidden;
}

.send-btn {
  width: 10%;
}

.roombtn {
  background-color: #d0a159;
  border-radius: 0;
  color: black;
  font-size: 1.1rem;
  width: 100%;
  height: 50px;
}

.massege_container {
  padding: 10px 0px 10px 0px !important;
  background-color: black;
  color: #a2a2a2;
  height: calc(100vh - 120px);
}

.control_area {
  background-color: black;
  color: #303133;
  overflow-y: scroll !important;
  height: 100vh;
}

.chat-input {
  background-color: #242424;
  color: #a2a2a2;
  height: 50px;
  border: none;
  width: 70%;
  padding-left: 20px;
}

.chat-input::placeholder {
  color: #a2a2a2;
}

.el-footer {
  padding: 10px !important;
}

.footer {
  position: absolute;
  bottom: 0px;
  width: calc(100% - 200px);
  background: #eee;
  height: auto !important;
  background-color: black;
  color: #a2a2a2;
  margin-bottom: 20px;
  margin-left: 20px;
}

.chat_body {
  height: calc(100% - 74px);
  overflow-y: scroll;
}
</style>