<!-- components/RollingNumber.vue -->
<script setup>
import { ref, onMounted, watch } from "vue";
import { CountUp } from "countup.js";
import { Odometer } from "vue3-odometer";

const props = defineProps({
  value: {
    type: Number,
    required: true,
  },
  duration: {
    type: Number,
    default: 2000, // ms
  },
  decimals: {
    type: Number,
    default: 2,
  },
  separator: {
    type: String,
    default: ",",
  },
  type: {
    type: String,
    default: "countup", // 'countup' | 'odometer' | 'classic'
  },
  unit: {
    type: String,
    default: "",
  },
});

const countupRef = ref(null);
let countUp = null;
const classicValue = ref(0);

const animateClassic = () => {
  const start = classicValue.value;
  const end = props.value;
  const startTime = performance.now();

  const animate = (now) => {
    const elapsed = now - startTime;
    const progress = Math.min(elapsed / props.duration, 1);
    const current = start + (end - start) * progress;
    classicValue.value = parseFloat(current.toFixed(props.decimals));
    if (progress < 1) requestAnimationFrame(animate);
  };

  requestAnimationFrame(animate);
};

onMounted(() => {
  if (props.type === "countup") {
    countUp = new CountUp(countupRef.value, props.value, {
      duration: props.duration / 1000,
      decimalPlaces: props.decimals,
      separator: props.separator,
    });
    countUp.start();
  } else if (props.type === "classic") {
    animateClassic();
  }
});

watch(
  () => props.value,
  (newVal) => {
    classicValue.value = newVal;
    countUp.update(newVal);
  }
);
</script>

<template>
  <div class="rolling-number">
    <template v-if="type === 'countup'">
      <span ref="countupRef" />
      <span style="margin-left: 0.5rem">{{ unit }}</span>
    </template>

    <template v-else-if="type === 'odometer'">
      <Odometer
        :value="props.value"
        :format="`(,ddd).${'d'.repeat(decimals)}`"
        class="odometer"
      />
    </template>

    <template v-else>
      <span>{{
        classicValue.toLocaleString(undefined, { minimumFractionDigits: decimals })
      }}</span>
    </template>
  </div>
</template>

<style scoped>
.rolling-number {
  font-size: 1.875rem;
}
</style>
