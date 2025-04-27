<template>
  <div class="chart3">
    <div class="chart-box" v-for="(item, index) in chartData" :key="index">
      <p>{{ item.name }}</p>
      <div class="chart-bar" :ref="(el) => (chartRefs[index] = el)"></div>
      <div class="data">
        <ul>
          <li>
            <p style="margin-bottom: 8px;">面积</p>
            <p>{{ item.area }} <span>万/㎡</span></p>
          </li>

          <li>
            <p style="margin-bottom: 8px;">空置率</p>
            <p>
              <span style="color: #ebb242; font-weight: bold">{{ item.vacancy }}%</span>
            </p>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script setup>
import * as echarts from "echarts";
import { nextTick, onMounted, ref } from "vue";

const chartRefs = [];
const chartData = [
  { name: "温江旭辉Cmall", area: 12, vacancy: 12.3 },
  { name: "高新未来广场", area: 85, vacancy: 25.6 },
  { name: "滨江中心", area: 108, vacancy: 7.8 },
];

const createOption = (item) => ({
  tooltip: {
    trigger: "item",
    formatter: ({ name, value }) =>
      `${item.name}<br/>${
        name === "空置率" ? `空置率: ${value}%` : `面积: ${item.area}㎡`
      }`,
    backgroundColor: "rgba(0,0,0,0.7)",
    textStyle: { color: "#fff" },
  },
  series: [
    {
      type: "pie",
      radius: ["60%", "80%"],
      center: ["50%", "50%"],
      label: { show: false },
      labelLine: { show: false },
      animation: true,
      animationType: "expansion",
      animationDuration: 1000,
      animationEasing: "cubicOut",
      data: [
        {
          value: item.vacancy,
          name: "空置率",
          itemStyle: { color: "#EBB242" },
        },
        {
          value: 100 - item.vacancy,
          name: "面积",
          itemStyle: {
            color: new echarts.graphic.LinearGradient(1, 0, 0, 1, [
              { offset: 0, color: "#1E93FF" },
              { offset: 1, color: "rgba(30,147,255,0.2)" },
            ]),
          },
        },
      ],
    },
  ],
});

onMounted(() => {
  nextTick(() => {
    chartData.forEach((item, index) => {
      const chart = echarts.init(chartRefs[index]);
      chart.setOption(createOption(item));
      chart.resize();
      const observer = new ResizeObserver(() => {
        chart && chart.resize();
      });
      observer.observe(chartRefs[index]);
      setTimeout(() => {
        chart.resize();
      }, 100);

      window.addEventListener("resize", () => chart.resize());
    });
  });
});
</script>
<style scoped lang="less">
.chart3 {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 100%;
  height: calc(100%);
  color: #fff;
  padding: 23px 0;
  padding-top: 43px;
}

.chart-box {
  width: 33%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  text-align: center;
  padding: 20px 0 0 0;
  p {
    font-size: 14px;
    margin-bottom: 4px;
    white-space: nowrap;
    text-align: left;
  }

  .chart-bar {
    width: 80px;
    height: 80px;
    margin: 0 auto;
  }

  .data {
    margin-top: 4px;

    ul {
      list-style: none;
      padding: 0;
      margin: 2px 0;
      font-size: 12px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      li {
        line-height: 1.4;
        margin-right: 23px;

        &:last-child {
          margin-right: 0;
        }
        p {
          font-family: Microsoft YaHei UI, Microsoft YaHei UI;
          font-weight: 600;
          font-size: 16px;
          font-weight: bold;
          line-height: 19px;
          margin-bottom: 0;
        }
        span {
          color: rgba(255, 255, 255, 0.5);
          font-weight: 400;
        }
      }
    }
  }
}
</style>
