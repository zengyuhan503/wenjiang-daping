<template>
  <div class="chart6" ref="chartRef"></div>
</template>

<script setup>
import * as echarts from "echarts";
import { onMounted, ref, nextTick, onBeforeUnmount } from "vue";

const chartRef = ref(null);
let chartInstance = null;

const initChart = () => {
  if (!chartRef.value) return;

  chartInstance = echarts.init(chartRef.value);

  const option = {
    xAxis: {
      type: "category",
      data: ["2010", "2011", "2012", "2013", "2014"],
      boundaryGap: true, // 柱状图推荐保留留白，false 会贴着边，如果你希望从最左开始可以改为 false
      axisLine: {
        lineStyle: { color: "#ffffff" },
      },
      axisLabel: {
        color: "#FFFFFF",
      },
    },
    
    yAxis: {
      type: "value",
      axisLine: {
        show: false,
      },
      axisLabel: {
        color: "#E6F7FF",
      },
      splitLine: {
        lineStyle: {
          type: "dashed",
          color: "rgba(230,247,255,0.2)",
        },
      },
    },
    series: [
      {
        name: "数据",
        type: "bar",
        data: [120, 200, 150, 80, 70],
        barWidth: "40%",
        itemStyle: {
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
            { offset: 0, color: "#18B2FF" },
            { offset: 1, color: "rgba(24,178,255,0.2)" },
          ]),
          borderRadius: [4, 4, 0, 0],
        },
      },
    ],
    grid: {
      left: "40px",
      right: "5%",
      bottom: "5%",
      top: "25%",
      containLabel: true,
    },
    tooltip: {
      trigger: "axis",
      axisPointer: {
        type: "shadow",
      },
    },
  };

  chartInstance.setOption(option);

  const observer = new ResizeObserver(() => {
    chartInstance && chartInstance.resize();
  });
  observer.observe(chartRef.value);
  setTimeout(() => chartInstance.resize(), 100);
  window.addEventListener("resize", () => chartInstance.resize());
};

onMounted(() => {
  nextTick(() => {
    initChart();

    window.addEventListener("resize", resizeChart);
  });
});

const resizeChart = () => {
  chartInstance && chartInstance.resize();
};

onBeforeUnmount(() => {
  window.removeEventListener("resize", resizeChart);
  chartInstance && chartInstance.dispose();
});
</script>

<style scoped>
.chart6 {
  width: 100%;
  height: 100%;
}
</style>
