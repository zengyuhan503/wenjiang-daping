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
  const showCount = 5;

  const option = {
    animation: true,
    animationDuration: 2000, // 初次渲染动画
    animationDurationUpdate: 2000, // 👈 滚动后新柱子的动画时间
    animationEasingUpdate: "cubicOut", // 👈 动画曲线，可换 'bounceOut' 看起来像跳出来
    xAxis: {
      type: "category",
      data: [
        "珠江国际中",
        "珠江国际中",
        "珠江国际中",
        "珠江国际中",
        "珠江国际中",
        "珠江国际中",
        "珠江国际中",
        "珠江国际中",
      ],
      boundaryGap: true,
      boundaryGap: true,
      axisTick: { show: false }, // 隐藏刻度线
      axisLine: {
        lineStyle: { color: "#ffffff" },
      },
      axisLabel: {
        interval: 0, // 强制显示所有刻度标签
        rotate: 0,
        margin: 10,
        fontSize: "14px",
        formatter: function (value) {
          return value.length > 4 ? value.slice(0, 4) + "…" : value;
        },
        color: "#FFFFFF",
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
        moveHandleSize: 0,
        borderRadius: 10,
        height: 12,
        bottom: 0,
        start: 0,
        zoomLock: true,
        endValue: showCount, // 设置初始显示的数量,
        brushSelect: false,
        fillerColor: new echarts.graphic.LinearGradient(0, 0, 1, 0, [
          { offset: 0, color: "#18B2FF" },
          { offset: 1, color: "rgba(24,178,255,0.2)" },
        ]),
        backgroundColor: "transparent",
        borderColor: "transparent",
        xAxisIndex: 0,
        handleStyle: {
          borderRadius: 10, // 虽然手柄隐藏，但仍可设置圆角
          color: "#fff",
          borderColor: "#999",
        },
        left: 0,
        right: 0,
        borderRadius: 5,
      },
      {
        type: "inside", // 不显示滑动条，仅通过滚轮控制
        xAxisIndex: 0,
        start: 0,
        end: 30, // 初始视图范围
        zoomOnMouseWheel: false, // 禁止滚轮缩放
        moveOnMouseWheel: true, // 开启滚轮移动
        moveOnAxis: "x", // 仅沿 x 轴移动
        throttle: 0,
      },
    ],
    yAxis: {
      type: "value",
      axisLine: {
        show: false,
      },
      axisLabel: {
        fontSize: 14,
        color: "#ffffff",
        verticalAlign: "top", // 让数字与横线居中
        padding: [-5, 0, 0, 0],
        margin: 15,
      },
      splitLine: {
        lineStyle: {
          type: "dashed",
          color: "rgba(230,247,255,0.2)",
        },
      },
    },
    grid: {
      left: "20px",
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
  setTimeout(() => {
    chartInstance.resize();
    let options = {
      series: [
        {
          name: "数据",
          type: "bar",
          data: [120, 200, 150, 80, 70, 70, 70, 70],
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
    };
    chartInstance.setOption(options);
  }, 100);
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
