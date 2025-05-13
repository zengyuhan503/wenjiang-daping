<script setup>
import { ref, onMounted, watch } from "vue";
import RollingNumber from "./RollingNumber.vue";
const val = ref(50);
const upDate_type = ref("countup");
const timer = ref(null);
const isMounted = ref(false);
onMounted(() => {
  isMounted.value = true;
  setTimeout(() => {
    val.value = Math.floor(Math.random() * 100000); 
  },2000)
});
</script>
<template>
  <div class="screen-header">
    <div class="items">
      <div class="item">
        <p class="label">已入驻面积</p>
        <RollingNumber
          class="value"
          :value="val"
          :type="upDate_type"
          :decimals="2"
          unit="㎡"
        />
      </div>
      <div class="item">
        <p class="label">未入驻面积</p>
        <RollingNumber
          class="value"
          :value="val"
          :type="upDate_type"
          :decimals="2"
          unit="㎡"
        />
      </div>
      <div class="item">
        <p class="label">总面积</p>
        <RollingNumber
          class="value"
          :value="val"
          :type="upDate_type"
          :decimals="2"
          unit="㎡"
        />
      </div>
      <div class="item">
        <p class="label">入驻率</p>
        <RollingNumber
          class="value"
          :value="val"
          :type="upDate_type"
          :decimals="2"
          unit="%"
        />
      </div>
      <div class="item">
        <p class="label">入驻企业（含集群）</p>
        <RollingNumber
          class="value"
          :value="val"
          :type="upDate_type"
          :decimals="2"
          unit="户"
        />
      </div>
      <div class="item">
        <p class="label">在温注册企业</p>
        <RollingNumber
          class="value"
          :value="val"
          :type="upDate_type"
          :decimals="2"
          unit="户"
        />
      </div>
      <div class="item">
        <p class="label">在温注册率</p>
        <RollingNumber
          class="value"
          :value="val"
          :type="upDate_type"
          :decimals="2"
          unit="%"
        />
      </div>
      <div class="item">
        <p class="label">平均租金</p>
        <RollingNumber
          class="value"
          :value="val"
          :type="upDate_type"
          :decimals="2"
          unit="元/㎡"
        />
      </div>
    </div>
  </div>
</template>

<style lang="less">
.screen-header {
  padding-top: 2.4375rem;
  .items {
    padding: 0 1.4375rem;
    display: grid;
    grid-template-columns: repeat(8, 1fr);
    gap: 1.4375rem;
    .item {
      width: 100%;
      background: url("../assets/image/section1-back.png") no-repeat;
      background-size: 100% 100%;

      display: flex;
      justify-content: space-between;
      flex-direction: column;
      .label {
        font-family: Microsoft YaHei UI, Microsoft YaHei UI;
        font-weight: 600;
        font-size: 1.25rem;
        height: 3rem;
        color: #ffffff;
        letter-spacing: 1px;
        text-align: center;
        line-height: 3rem;
        &::after {
          content: "";
          display: block;
          width: 100%;
          height: 2px;
          background: linear-gradient(
            90deg,
            rgba(37, 145, 255, 0) 0%,
            #2591ff 50%,
            rgba(37, 145, 255, 0) 100%
          );
          border-radius: 50%;
        }
      }
      .value {
        font-family: Arial, Arial;
        font-weight: bold;
        font-size: 1.875rem;
        text-align: center;
        height: 56px;
        line-height: 56px;
        font-style: normal;
        text-transform: none;
        background: linear-gradient(90deg, #ffffff 0%, #2591ff 100%);
        -webkit-background-clip: text;
        background-clip: text;
        -webkit-text-fill-color: transparent;
        span:first-child {
          font-size: 1.5625rem;
        }
        span {
          font-size: 1.25rem;
        }
      }
    }
  }
}
</style>
