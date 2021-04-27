<template>
  <div class="single_message">
    <div class="avatar_block flex">
      <div class="avatar" v-if="message.username !== null">
        {{ message.username.charAt(0) }}
      </div>
      <div class="message">
        <h2 v-if="message.username !== null">
          {{ message.username }}
          <!-- <span class="time">{{message.time}}</span> -->
        </h2>
      </div>
    </div>
    <div>
      <div class="message">
        <p style="font-size: 0.83rem;">{{ message.message }}</p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Single_message",
  props: ["message"],
  data() {
    return {};
  },
};
</script>
<style lang="scss" scoped>

.message{
  background-color: #111111;
}
.single_message {
  padding: 10px 5px;

  .flex {
    display: flex;
    align-items: top;

    .avatar {
      height: 40px;
      width: 40px;
      background: white;
      border-radius: 50%;
      text-align: center;
      display: flex;
      justify-content: center;
      align-items: center;
      line-height: 30px;
      font-weight: 600;
      color: #000000;
      margin-right: 15px;
      margin-left: 15px;
      text-transform: uppercase;
    }
  }
}

.message {
  .time {
    font-size: 10px;
    color: #d2d2d2;
    font-weight: 600;
    margin-left: 5px;
  }

  h2 {
    color: #d2d2d2;
    font-size: 16px;
  }

  p {
    color: #d2d2d2;
    margin-left: 20px;
  }
}
</style>
