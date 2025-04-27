<template>
  <div class="chart2" ref="chartRef"></div>
</template>

<script setup>
import * as echarts from "echarts";
import { onMounted, ref, nextTick } from "vue";

const chartRef = ref(null);

const data = [
  { name: "租赁和商务服务业", value: 85 },
  { name: "项租赁和商务服务业租赁和商务服务业目B", value: 60 },
  { name: "项目C", value: 42 },
  { name: "项目D", value: 30 },
  { name: "项目E", value: 15 },
];

onMounted(() => {
  const chart = echarts.init(chartRef.value);

  const option = {
    animation: true,
    animationDuration: 1000,
    animationDurationUpdate: 800,
    animationEasing: "cubicOut",
    animationEasingUpdate: "cubicOut",

    tooltip: {
      trigger: "axis",
      axisPointer: { type: "shadow" },
      textStyle: {
        color: "#fff",
      },
    },
    grid: {
      left: "2%",
      right: "30px",
      bottom: "8px",
      top: "25%",
      containLabel: true,
    },
    xAxis: {
      type: "value",
      max: 100,
      axisLabel: {
        color: "#ffffff",
        formatter: function (value) {
          const max = 100; // 假设你知道最大值
          return value === max ? value + "     户" : value;
        },
      },
      splitLine: {
        lineStyle: {
          type: "dashed",
          color: "#444444",
        },
      },
    },
    yAxis: {
      type: "category",
      data: data.map((item) => item.name),
      axisLabel: {
        color: "#ffffff",
        fontSize: 14,
      },
      axisLine: {
        lineStyle: {
          color: "#ffffff",
        },
      },
      axisLabel: {
        formatter: function (value) {
          return value.length > 7 ? value.slice(0, 7) + "..." : value;
        },
      },
    },
    series: [
      {
        type: "bar",
        data: data.map((item) => item.value),
        barWidth: 16,
        itemStyle: {
          color: new echarts.graphic.LinearGradient(1, 0, 0, 0, [
            { offset: 0, color: "#21F3F3" },
            { offset: 1, color: "rgba(30,231,231,0.5)" },
          ]),
        },
      },
    ],
  };
  nextTick(() => {
    chart.setOption(option);
    chart.resize();
    const observer = new ResizeObserver(() => {
      chart && chart.resize();
    });
    observer.observe(chartRef.value);
    setTimeout(() => {
      chart.resize();
    }, 100);
    window.addEventListener("resize", () => chart.resize());
  });
});
</script>

<style scoped lang="less">
.chart2 {
  width: 100%;
  height: 100%;
}
</style>
