<script setup>
import { onMounted } from "vue";
import * as echarts from "echarts";
import mapjson from "../assets/map.json";
import mapJsonSvg from "../assets/wenjiang.svg";
import mapsvg from "../assets/mapsvg.svg";
import graphicsvg from "../assets/map.png";
import axios from "axios";

onMounted(() => {
  axios.get(mapjson).then((res) => {
    console.log(res);
    let data = res.data;

    var chartDom = document.getElementById("main");
    var myChart = echarts.init(chartDom);
    echarts.registerMap("wenjiang", mapjson);
    // SVG椭圆路径 (cx=50 cy=50 rx=40 ry=30)
    const ellipsePath = "M50,50 m-40,0 a40,30 0 1,0 80,0 a40,30 0 1,0 -80,0";

    const markerData = [
      { name: "星河world科创城3", value: [103.695006, 30.887757] },
      { name: "光华公园站", value: [103.87, 30.71] },
      { name: "南熏大道", value: [103.85, 30.69] },
      { name: "天欣驾校西门训练基地", value: [103.91, 30.67] },
      { name: "浩旺产业园共耕工业园区", value: [103.9, 30.67] },
      { name: "温江站", value: [103.81, 30.63] },
    ];
    let options = {
      geo: {
        id: "geo",
        map: "wenjiang",
        region: [],
        select: {
          disabled: true,
        },
        zoom: 1,
        roam: true,
        itemStyle: {
          normal: {
            borderColor: "#00ffff", // 亮蓝色边框
            borderWidth: 3, // 边框宽度
            shadowBlur: 20, // 虚影范围
            shadowColor: "rgba(0, 255, 255, 0.8)", // 蓝色光晕
            shadowOffsetX: 0,
            shadowOffsetY: 0,
            areaColor: {
              type: "image",
              image: graphicsvg,
              repeat: "repeat",
            },
          },
          emphasis: {
            borderColor: "#00ffff", // 亮蓝色边框
            borderWidth: 3, // 边框宽度
            shadowBlur: 20, // 虚影范围
            shadowColor: "rgba(0, 255, 255, 0.8)", // 蓝色光晕
            shadowOffsetX: 0,
            shadowOffsetY: 0,
            areaColor: {
              type: "image",
              image: graphicsvg,
              repeat: "repeat",
            },
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
      },
      tooltip: {
        show: false,
      },
      series: [
        //=====1.静态椭圆形黄点=====
        {
          type: "scatter",
          coordinateSystem: "geo",
          symbol: `path://${ellipsePath}`,
          symbolSize: [15, 8], //宽度15，高度8
          symbolKeepAspect: false, //允许非等比缩放
          itemStyle: { color: "#ffcc00" },
          data: [...markerData],

          emphasis: {
            symbolSize: [20, 10], //悬停时放大尺寸
            itemStyle: { color: "#ffcc00" },
          },
        },

        //=====2.椭圆形涟漪动画=====
        {
          type: "effectScatter",
          coordinateSystem: "geo",
          showEffectOn: "emphasis",
          symbol: `path://${ellipsePath}!`, // 注意感叹号
          symbolSize: [20, 10], //悬停时放大尺寸
          symbolKeepAspect: false,
          rippleEffect: {
            brushType: "stroke",
            scale: 3, // 整体缩放倍数
            period: 3,
            trailLength: 0.5,
          },
          itemStyle: {
            color: "#ffcc00",
            opacity: 0, // 默认隐藏
          },
          emphasis: {
            itemStyle: {
              opacity: 1, // hover 时出现
            },
          },
          data: [...markerData],
        },
      ],
    };

    const defaultHighlightIndexes = [1, 0]; // 默认高亮的两个点
    let currentHighlightIndex = [];
    const tooltipMap = {}; // 存储 tooltip DOM 引用
    myChart.setOption(options);

    // 创建自定义 tooltip DOM
    function createCustomTooltips(currentHighlightIndex = []) {
      const container = document.getElementById("main");
      // 清除旧的
      document.querySelectorAll(".custom-tooltip").forEach((el) => el.remove());
      let indexs =
        currentHighlightIndex.length > 0
          ? currentHighlightIndex
          : defaultHighlightIndexes;
      indexs.forEach((index) => {
        const point = markerData[index];
        const pos = myChart.convertToPixel({ geoIndex: 0 }, point.value);

        const tooltip = document.createElement("div");
        tooltip.className = "tooltip";
        tooltip.innerHTML = `
          <div>
            <div class="name">${point.name}</div>
            <div class="before"></div>
          </div>`;
        tooltip.style.cssText = `
          position: absolute;
          left: ${pos[0]}px;
          top: ${pos[1] - 100}px;
        `;
        container.appendChild(tooltip);
        // ✨ 关键：获取宽度后重新设置 left，让它居中
        const width = tooltip.offsetWidth;
        const height = tooltip.offsetHeight;
        tooltip.style.top = `${pos[1] - height - 28}px `;
      });
    }

    // 高亮默认
    function setDefaultHighlight() {
      defaultHighlightIndexes.forEach((index) => {
        myChart.dispatchAction({
          type: "highlight",
          seriesIndex: 1,
          dataIndex: index,
        });
      });
    }

    // 清除所有高亮和 tooltip
    function clearAllHighlight() {
      markerData.forEach((_, index) => {
        myChart.dispatchAction({
          type: "downplay",
          seriesIndex: 1,
          dataIndex: index,
        });
      });
      document.querySelectorAll(".tooltip").forEach((el) => el.remove());
    }
    let initPixel = [];
    // 初始设置
    setTimeout(() => {
      setDefaultHighlight();
      createCustomTooltips();
    }, 0);

    // 鼠标交互
    myChart.on("mouseover", (params) => {
      if (params.seriesType === "effectScatter") {
        clearAllHighlight();
        myChart.dispatchAction({
          type: "highlight",
          seriesIndex: 1,
          dataIndex: params.dataIndex,
        });
        currentHighlightIndex = [params.dataIndex];
        console.log(currentHighlightIndex);
        createCustomTooltips(currentHighlightIndex);
      }
    });

    myChart.on("mouseout", (params) => {
      if (params.seriesType === "effectScatter") {
        clearAllHighlight();
        setDefaultHighlight();
        createCustomTooltips();
      }
    });
    // 地图缩放或拖动时更新 tooltip 位置
    myChart.on("georoam", (e) => {
      clearAllHighlight();
      createCustomTooltips();
    });
  });
});
</script>

<template>
  <div id="main" style="position: relative"></div>
</template>

<style lang="less">
#main {
  width: 740px;
  height: 740px;
  overflow: hidden;
}
.custom-tooltip {
  position: absolute;
  background: rgba(50, 50, 50, 0.85);
  color: #fff;
  padding: 6px 10px;
  border-radius: 4px;
  pointer-events: none;
  font-size: 12px;
  white-space: nowrap;
  z-index: 10;
  transform: translate(-50%, -100%);
}
.tooltip {
  position: absolute;
  transform: translate(-50%, -100%);
}
.tooltip > div {
  display: block;
  padding: 5px;
  background: rgba(255, 205, 69, 0.5);
  box-shadow: inset 0px 0px 20px 1px rgba(255, 205, 69, 0.65);
  border-radius: 0px 0px 0px 0px;
  border: 1px solid #ffcd45;
  .name {
    font-family: Microsoft YaHei UI, Microsoft YaHei UI;
    font-weight: 600;
    font-size: 16px;
    color: #ffffff;
    text-align: center;
  }
  .before {
    width: 1px;
    height: 58px;
    border-radius: 0px 0px 0px 0px;
    border: 2px solid;
    border-image: linear-gradient(180deg, rgba(255, 205, 69, 0), rgba(255, 205, 69, 1)) 2
      2;
    position: absolute;
    left: 50%;
    top: 100%;
    transform: translateX(-50%);
  }
}
</style>
