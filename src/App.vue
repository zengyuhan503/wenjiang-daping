<script setup>
import { onMounted, ref } from "vue";
import VScaleScreen from "v-scale-screen";
import Header from "./components/header.vue";
import ScreenHeader from "./components/screenHeader.vue";
import Chart1 from "./components/charts/chart1.vue";
import Chart2 from "./components/charts/chart2.vue";
import Chart3 from "./components/charts/chart3.vue";
import Chart4 from "./components/charts/chart4.vue";
import Chart5 from "./components/charts/chart5.vue";
import Chart6 from "./components/charts/chart6.vue";
import CenterChart from "./components/center_chart.vue";
const handleScreenAuto = () => {
  const designDraftWidth = 1920; //设计稿的宽度
  const designDraftHeight = 960; //设计稿的高度
  // 根据屏幕的变化适配的比例
  const scale =
    document.documentElement.clientWidth / document.documentElement.clientHeight <
    designDraftWidth / designDraftHeight
      ? document.documentElement.clientWidth / designDraftWidth
      : document.documentElement.clientHeight / designDraftHeight;
  // 缩放比例
  document.querySelector(
    "#screen"
  ).style.transform = `scale(${scale}) translate(-50%, -50%)`;
};

onMounted(() => {
  // handleScreenAuto();
  // // 绑定自适应函数   ---防止浏览器栏变化后不再适配
  // window.onresize = () => handleScreenAuto();
});
</script>

<template>
  <v-scale-screen width="1920" height="1080">
    <Header />
    <div class="screen-root">
      <section>
        <ScreenHeader />
      </section>
      <div class="screen-content">
        <div class="screen-content-left">
          <div class="screen-content-left-item">
            <div class="screen-title">
              <span>实时入驻情况</span>
            </div>
            <Chart1 />
          </div>
          <div class="screen-content-left-item">
            <div class="screen-title">
              <span>企业类型</span>
              <span style="color: #fff;font-weight: 600;">总计：2210</span>
            </div>
            <Chart2 />
          </div>
          <div class="screen-content-left-item">
            <div class="screen-title">
              <span>招租信息需求单</span>
            </div>
            <Chart3 />
          </div>
        </div>
        <div class="screen-content-center">
          <CenterChart />
        </div>
        <div class="screen-content-right">
          <div class="screen-content-right-item">
            <div class="screen-title">
              <span>入驻趋势</span>
            </div>
            <Chart4 />
          </div>
          <div class="screen-content-right-item">
            <div class="screen-title">
              <span>租金趋势</span>
              <span>单位：元</span>
            </div>
            <Chart5 />
          </div>
          <div class="screen-content-right-item">
            <div class="screen-title">
              <span>规上企业</span>
              <span style="color: #e6f7ff">单位：户</span>
            </div>
            <Chart6 />
          </div>
        </div>
      </div>
    </div>
  </v-scale-screen>
</template>

<style lang="less">
/*
  除了设计稿的宽高是根据您自己的设计稿决定以外，其他复制粘贴就完事
*/
.screen-root {
  height: calc(100% - 80px);
  width: 100%;
  .screen {
    display: inline-block;
    width: 1920px; //设计稿的宽度
    height: 960px; //设计稿的高度
    transform-origin: 0 0;
    position: absolute;
    left: 50%;
    top: 50%;
  }
  .screen-content {
    padding: 1.5rem;
    width: 100%;
    height: calc(100% - 120px);
    display: flex;
    justify-content: space-between;
    align-items: flex-start;

    .screen-content-left,
    .screen-content-right {
      width: 30%;
    }
    .screen-content-left-item,
    .screen-content-right-item {
      width: 100%;
      height: 263px;
      background: url("./assets/image/chat-bg.png") no-repeat;
      background-size: 100% 100%;
      position: relative;
      margin-bottom: 23px;
      &:last-child {
        margin-bottom: 0;
      }
      .screen-title {
        position: absolute;
        width: 100%;
        height: 40px;
        background: url("./assets/image/title-bg.png") no-repeat;
        background-size: 100% 100%;
        background-position: 0 0;
        text-align: left;
        padding-left: 2.8125rem;
        padding-right: 0.625rem;
        display: flex;
        justify-content: space-between;
        align-items: center;
        span {
          font-size: 1rem;
          color: #e6f7ff;
          font-weight: 400;
          &:first-child {
            font-weight: bold;
            color: #fff;
          }
          &:last-child {
            text-shadow: 0px 4px 10px rgba(94, 251, 253, 0.2);
          }
        }
      }
      .chart1 {
        width: 100%;
        height: 100%;
      }
    }
  }
}
</style>

<style>
html,
body,
#app {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  font-size: 14px;
}
.screen-wrapper {
  background: url("./assets/image/man-back.png") no-repeat;
  background-size: 100% 100%;
}
p {
  margin: 0;
  padding: 0;
}
div {
  box-sizing: border-box;
}
</style>
