<template>
  <div class="chartCenter" ref="chartRef"></div>
</template>

<script setup>
import * as echarts from "echarts";
import { ref, onMounted, nextTick } from "vue";
let chartCenter = null;
const chartRef = ref(null);
const chart_data = ref([
  { name: "珠江国际中心", registerRate: 99.17, entryRate: 4.81 },
  { name: "新光天地", registerRate: 4.81, entryRate: 19.17 },
  { name: "德坤新天地", registerRate: 19.17, entryRate: 10.5 },
  { name: "成都东创科", registerRate: 10.5, entryRate: 12.3 },
  { name: "新时泛亚大厦1", registerRate: 12.3, entryRate: 8.2 },
  { name: "新时泛亚大厦2", registerRate: 12.3, entryRate: 8.2 },
  { name: "新时泛亚大厦3", registerRate: 12.3, entryRate: 8.2 },
  { name: "新时泛亚大厦", registerRate: 12.3, entryRate: 8.2 },
  { name: "新时泛亚大厦", registerRate: 12.3, entryRate: 8.2 },
  { name: "新时泛亚大厦", registerRate: 12.3, entryRate: 8.2 },
]);

const option = {
  grid: {
    bottom: "15%",
    top: "25%",
  },
  tooltip: {
    trigger: "axis",
    axisPointer: {
      type: "shadow",
    },
  },
  dataZoom: [
    // ←← 这一段是新加的
    {
      type: "slider",
      show: true,
      showDetail: false,
      showDataShadow: false,
      handleSize: 0,
      height: 12,
      bottom: 0,
      start: 0,
      end: 45,
      fillerColor: "#0a477d",
      backgroundColor: "transparent",
      borderColor: "transparent",
      xAxisIndex: 0,
    },
  ],
  legend: {
    data: ["注册率", "入驻率"],
    itemWidth: 12,
    itemHeight: 12,
    right: "15px",
    top: "10px",
    textStyle: {
      color: "#FFFFFF", // 设置文字颜色
      fontSize: 12, // 可选：设置字体大小
    },
  },
  xAxis: {
    type: "category",
    data: chart_data.value.map((item) => item.name),
    axisLabel: {
      interval: 0, // 强制显示所有刻度标签
      rotate: 0,
      margin: 10,
      fontSize: "12px",
      formatter: function (value) {
        return value.length > 5 ? value.slice(0, 5) + "…" : value;
      },
    },

    axisLine: {
      lineStyle: {
        color: "#FFFFFF",
      },
    },
  },
  yAxis: {
    type: "value",
    max: 100,
    min: 0,
    axisLabel: {
      formatter: "{value}%",
    },
    splitLine: {
      lineStyle: {
        type: "dashed",
        color: "#E6F7FF",
      },
    },
  },
  grid: {
    left: "13px",
    right: "23px",
    bottom: "23px",
    top: "25%",
    containLabel: true,
  },
  series: [
    {
      name: "注册率",
      type: "bar",
      data: chart_data.value.map((item) => item.registerRate),

      barWidth: 16,
      itemStyle: {
        color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
          { offset: 0, color: "#18B2FF" },
          { offset: 1, color: "rgba(24,178,255,0.2)" },
        ]),
      },
    },
    {
      name: "入驻率",
      type: "bar",
      barWidth: 16,
      data: chart_data.value.map((item) => item.entryRate),
      itemStyle: {
        color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
          { offset: 0, color: "#21F3F3" },
          { offset: 1, color: "rgba(30,231,231,0.2)" },
        ]),
      },
    },
  ],
  animation: true,
  animationDuration: 2000, // 初次渲染动画
  animationDurationUpdate: 2000, // 👈 滚动后新柱子的动画时间
  animationEasingUpdate: "cubicOut", // 👈 动画曲线，可换 'bounceOut' 看起来像跳出来
};
const initChart = () => {
  chartCenter = echarts.init(chartRef.value);
  nextTick(() => {
    chartCenter.setOption(option);
    const observer = new ResizeObserver(() => {
      chart_data && chartCenter.resize();
    });
    observer.observe(chartRef.value);
    setTimeout(() => {
      chartCenter.resize();
    }, 100);
    window.addEventListener("resize", () => chartCenter.resize());
  });
};
onMounted(() => {
  initChart();
});
</script>

<style lang="less" scoped>
.chartCenter {
  width: 100%;
  height: 100%;
}
</style>
