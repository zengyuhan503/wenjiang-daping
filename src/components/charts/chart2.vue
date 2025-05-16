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
  { name: "项目C", value: 85 },
  { name: "项目D", value: 30 },
  { name: "项目E", value: 85 },
  { name: "项目E", value: 85 },
  { name: "项目E", value: 85 },
  { name: "项目E", value: 85 },
  { name: "项目E", value: 85 },
  { name: "项目E", value: 85 },
  { name: "项目E", value: 85 },
  { name: "项目E", value: 85 },
  { name: "项目E", value: 85 },
  { name: "项目E", value: 85 },
  { name: "项目E", value: 85 },
  { name: "项目E", value: 85 },
  { name: "项目E", value: 85 },
];

onMounted(() => {
  const chart = echarts.init(chartRef.value);

  const showCount = 4;
  const option = {
    animation: true,
    animationDuration: 2000, // 初次渲染动画
    animationDurationUpdate: 2000, // 👈 滚动后新柱子的动画时间
    animationEasingUpdate: "cubicOut", // 👈 动画曲线，可换 'bounceOut' 看起来像跳出来

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
        orient: "vertical", // 设置为纵向
        show: true,
        showDetail: false,
        showDataShadow: false,
        handleSize: 0,
        moveHandleSize: 0,
        borderRadius: 10,
        height: "100%",
        width: 14,
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
        yAxisIndex: 0,
        handleStyle: {
          borderRadius: 10, // 虽然手柄隐藏，但仍可设置圆角
          color: "#fff",
          borderColor: "#999",
        },
        borderRadius: 5,
      },
      {
        type: "inside", // 不显示滑动条，仅通过滚轮控制
        yAxisIndex: 0,
        start: 0,
        end: 30, // 初始视图范围
        zoomOnMouseWheel: false, // 禁止滚轮缩放
        moveOnMouseWheel: true, // 开启滚轮移动
        moveOnAxis: "y", // 仅沿 x 轴移动
        throttle: 0,
      },
    ],
    grid: {
      left: "20px",
      right: "5%",
      bottom: "5%",
      top: "25%",
      containLabel: true,
    },
    xAxis: {
      type: "value",
      max: 100,
      axisLabel: {
        color: "#ffffff",
        fontSize: 14,
        formatter: function (value) {
          const max = 100; // 假设你知道最大值
          return value === max ? value + "     户" : value;
        },
      },
      splitLine: {
        lineStyle: {
          type: "dashed",
          color: " rgba(230,247,255,0.2)",
        },
      },
    },
    yAxis: {
      type: "category",
      data: data.map((item) => item.name),
      axisTick: { show: false }, // 隐藏刻度线
      axisLabel: {
        color: "#ffffff",
        fontSize: 14,
        verticalAlign: "top", // 让数字与横线居中
        padding: [-5, 0, 0, 0],
        margin: 15,
      },
      axisLine: {
        lineStyle: {
          color: "#BAE7FF",
        },
      },
      axisLabel: {
        formatter: function (value) {
          return value.length > 7 ? value.slice(0, 7) + "..." : value;
        },
      },
    },
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
      let options = {
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
      chart.setOption(options);
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
