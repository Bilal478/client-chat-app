<template>
  <li class=".list">
    <div class="user">U</div>
    <span>{{user}}</span>
  </li>
</template>
<script>
export default {
  name: "single_user",
  props: ["user"]
};
</script>
<style lang="scss" scoped>

.user{
  border-radius: 50%;
  height: 40px;
  width: 40px;
  background-color: white;
  color: black;
  display: inline-block;
  text-align: center;
  font-weight: bold;
}
li {
  font-size: 14px;
  line-height: 40px;
  span {
    padding-left: 14px;
  }
}
</style>
