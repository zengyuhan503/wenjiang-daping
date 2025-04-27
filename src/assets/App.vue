<script setup>
import { onMounted } from "vue";
import * as echarts from "echarts";
import mapjson from "./assets/map.json";
import mapsvg from "./assets/Vector.svg";
import axios from "axios";

onMounted(() => {
  var chartDom = document.getElementById("main");
  var myChart = echarts.init(chartDom);
  echarts.registerMap("wenjiang", mapjson);
  var geoCoordMap = echarts.getMap("wenjiang").geoJson.features[0].geometry.coordinates;
  var geoBBox = echarts.util.map(geoCoordMap, function (coord) {
    return coord[0];
  });
  var bbox = echarts.graphic.clipRectByRect(echarts.graphic.getBoundingRect(geoBBox), {
    x: 0,
    y: 0,
    width: myChart.getWidth(),
    height: myChart.getHeight(),
  });

  // 计算背景图的位置和尺寸
  var imageWidth = bbox.width;
  var imageHeight = bbox.height;
  var imageX = bbox.x;
  var imageY = bbox.y;

  console.log("Image Width:", imageWidth);
  console.log("Image Height:", imageHeight);
  console.log("Image X:", imageX);
  console.log("Image Y:", imageY);
  let options = {
    geo: {
      map: "wenjiang",
      region: [],
      select: {
        disabled: true,
      },
      itemStyle: {
        normal: {
          areaColor: "transparent", // 设置默认区域颜色
          borderColor: "#444", // 设置默认边框颜色
          borderWidth: 1, // 设置默认边框宽度
        },
        emphasis: {
          areaColor: "transparent", // 设置 hover 区域颜色，与默认颜色相同
          borderColor: "#444", // 设置 hover 边框颜色，与默认颜色相同
          borderWidth: 1, // 设置 hover 边框宽度，与默认宽度相同
        },
      },
      label: {
        normal: {
          show: false, // 禁用默认状态下的标签显示
        },
        emphasis: {
          show: false, // 禁用 hover 状态下的标签显示
        },
      },
      roam: true,
    },
    graphic: {
      elements: [
        {
          type: "image",
          style: {
            image: mapsvg,
            x: 0,
            y: 0,
            width: 626,
            height: "546",
          },
        },
      ],
    },
  };

  myChart.setOption(options);
});
</script>

<template>
  <div id="main"></div>
</template>

<style>
#main {
  width: 626px;
  height: 546px;
}
</style>
