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
const showCount = 3;

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
      moveHandleSize: 0,
      borderRadius: 10,
      height: 12,
      bottom: 0,
      start: 0,
      zoomLock: true,
      endValue: showCount, // 设置初始显示的数量,
      left: 0,
      right: 0,
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
  legend: {
    data: ["注册率", "入驻率"],
    itemWidth: 12,
    itemHeight: 12,
    right: "18px",
    itemGap: 30,
    top: "10px",
    textStyle: {
      color: "#FFFFFF", // 设置文字颜色
      fontSize: 14, // 可选：设置字体大小
    },
  },
  xAxis: {
    type: "category",
    data: chart_data.value.map((item) => item.name),
    axisTick: { show: false }, // 隐藏刻度线
    axisLabel: {
      interval: 0, // 强制显示所有刻度标签
      rotate: 0,
      fontSize: "14px",
      formatter: function (value) {
        return value.length > 6 ? value.slice(0, 6) + "…" : value;
      },
      color: "#FFFFFF",
    },

    axisLine: {
      lineStyle: {
        color: "#BAE7FF",
        showMinLine: false,
      },
    },
  },
  yAxis: {
    type: "value",
    max: 100,
    min: 0,
    axisLabel: {
      formatter: "{value}%",
      color: "#E6F7FF",
      fontSize: "14px",
      verticalAlign: "top", // 让数字与横线居中
      padding: [-5, 0, 0, 0],
      margin: 15,
    },
    splitLine: {
      lineStyle: {
        type: "dashed",
        color: " rgba(230,247,255,0.2)",
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
      let options = {
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
      };
      chartCenter.setOption(options);
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
