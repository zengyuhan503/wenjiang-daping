<template>
  <div class="chart5" ref="chartRef"></div>
</template>
<script setup>
import * as echarts from "echarts";
import { nextTick, onMounted, ref } from "vue";

const chartData = [
  { year: "2018", value: 45 },
  { year: "2019", value: 60 },
  { year: "2020", value: 55 },
  { year: "2021", value: 70 },
  { year: "2022", value: 80 },
  { year: "2023", value: 90 },
];
const chartRef = ref(null);

const option = {
  tooltip: {
    trigger: "axis",
    formatter: (params) => {
      const item = params[0];
      return `${item.name}<br/>占比：${item.value}`;
    },
    backgroundColor: "rgba(0,0,0,0.7)",
    textStyle: {
      color: "#fff",
      fontSize: 12,
    },
  },
  grid: {
    left: "5%",
    right: "5%",
    bottom: "5%",
    top: "25%",
    containLabel: true,
  },
  xAxis: {
    type: "category",
    data: chartData.map((d) => d.year),
    axisLine: { lineStyle: { color: "#fff" } },
    axisLabel: { color: "#fff" },
    boundaryGap: false,
  },
  yAxis: {
    type: "value",
    max: 100,
    min: 0,
    axisLabel: {
      formatter: "{value}%",
      color: "#fff",
    },
    axisLine: { lineStyle: { color: "#fff" } },
    splitLine: { lineStyle: { type: "dashed", color: "#444" } },
  },
  series: [
    {
      type: "line",
      data: chartData.map((d) => d.value),
      smooth: true,
      symbol: "circle",
      symbolSize: 10,
      lineStyle: {
        color: "#50C5FF",
        width: 2,
      },
      itemStyle: {
        color: "#50C5FF",
        shadowColor: "#50C5FF",
        shadowBlur: 20,
      },
      emphasis: {
        scale: true,
      },
      areaStyle: {
        color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
          {
            offset: 0,
            color: "rgba(80,197,255,0.2)",
          },
          {
            offset: 1,
            color: "rgba(80,197,255,0)",
          },
        ]),
      },
      animation: true,
      animationDuration: 1000,
      animationEasing: "cubicOut",
    },
  ],
};
onMounted(() => {
  nextTick(() => {
    const chart = echarts.init(chartRef.value);
    chart.setOption(option);
    chart.resize();
    const observer = new ResizeObserver(() => {
      chart && chart.resize();
    });
    observer.observe(chartRef.value);
    setTimeout(() => chart.resize(), 100);
    window.addEventListener("resize", () => chart.resize());
  });
});
</script>
<style scoped lang="less">
.chart5 {
  width: 100%;
  height: 100%;
}
</style>
