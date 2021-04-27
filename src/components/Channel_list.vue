<template>
  <div class="channels">
    <p class="subtitle">Channels</p>
    <div class="channel_list">
      <ul>
        <li
          class="t"
          style="cursor: pointer;"
          v-for="(single_channel, key) in list"
          :key="key"
          v-on:click="switchroom(single_channel)"
          v-bind:class="{ current_class: single_channel === active }"
        >
          <span class="channel_nick">{{ single_channel }}</span>
          <i class="fas fa-chevron-right" style="float: right; margin-top: 13px;" ></i>
        </li>
      </ul>
    </div>
  </div>
</template>
<style lang="scss" scoped>
.channels {
  background-color: black;
  color: #d2d2d2;
}
.subtitle {
  padding: 6px 10px;
  font-weight: bolder;
  font-size: 2rem;
  color: #d2d2d2;
}

.t {
  margin: 5px 0;
}

.channel_list {
  ul {
    margin: 0px;
    padding: 0px;

    li {
      list-style: none;
      line-height: 40px;
      font-size: 14px;
      padding: 10px;

      .channel_nick {
        margin-left: 10px;
        font-weight: bold;
        font-size: 1rem;
      }
    }

    .current_class {
      background: #242424;
      color: #d0a159;
    }
  }
}
</style>
<script>
export default {
  name: "Channel_list",
  props: ["list", "active"],
  data() {
    return {
      joined_rooms: [],
    };
  },
  methods: {
    switchroom(newroom) {
      this.$store.dispatch("clearnthemessages");
      socket.emit("switchrooms", newroom);
    },
  },
};
</script>

