<script setup>
import DateTimeDisplay from "./DateTimeDisplay.vue";
</script>

<template>
  <div class="header">
    <div class="addres">
      <img src="../assets/image/addres-icon.png" alt="" />
      <p class="text">温江区</p>
    </div>
    <div class="title">成都市温江区商务局智慧楼宇数据监控中心</div>
    <DateTimeDisplay />
  </div>
</template>
<style lang="less" scoped>
.header {
  height: 72px;
  background-image: url("../assets/image/header.png");
  background-size: 100% 100%;
  background-repeat: no-repeat;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  padding: 0 2.0625rem;
  position: relative;

  .addres {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 50px;
    img {
      width: 14px;
      margin-right: 0.25rem;
    }
    .text {
      font-family: Microsoft YaHei UI, Microsoft YaHei UI;
      font-weight: 600;
      font-size: 1.125rem;
      color: #55c6ff;
    }
  }
  .title {
    height: 3.875rem;
    font-weight: normal;
    font-size: 32px;
    letter-spacing: 3px;
    text-align: center;
    font-size: 1.75rem;
    font-weight: bold;
    background: linear-gradient(270deg, #a8e4ff 0%, #ffffff 100%);
    -webkit-background-clip: text;
    background-clip: text;
    -webkit-text-fill-color: transparent;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }
  .orther {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 3.125rem;
    p {
      font-family: Arial, Arial;
      font-weight: 400;
      font-size: 1.125rem;
      color: #55c6ff;
      text-align: right;
      font-style: normal;
      text-transform: none;
    }
  }
}
</style>
