<template>
  <div class="orther">
    <p class="date">{{ currentDate }}</p>
    <p class="weeks" style="margin: 0 1.25rem">{{ currentWeekday }}</p>
    <p class="timer">{{ currentTime }}</p>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import dayjs from "dayjs";

const currentDate = ref("");
const currentTime = ref("");
const currentWeekday = ref("");

const weekdays = ["星期日", "星期一", "星期二", "星期三", "星期四", "星期五", "星期六"];

let timer = null;

const updateTime = () => {
  const now = dayjs();
  currentDate.value = now.format("YYYY-MM-DD");
  currentTime.value = now.format("HH:mm:ss");
  currentWeekday.value = weekdays[now.day()];
};

onMounted(() => {
  updateTime();
  timer = setInterval(updateTime, 1000);
});

onUnmounted(() => {
  clearInterval(timer);
});
</script>

<style scoped>
.orther {
  display: flex;
  align-items: center;
  font-size: 1.25rem;
  font-weight: bold;
  color: #55c6ff;
}
.date,
.timer,
.weeks {
  margin: 0;
}
</style>
