<template>
  <div id="chart">
    <apexchart
      type="area"
      height="350"
      :options="chartOptions"
      :series="series"
    ></apexchart>
  </div>
</template>

<script setup>
import { ref } from "vue";

// props
const props = defineProps({
  dataProps: Object,
});

// Khai báo các biến
const dates = ref(props.dataProps.dates);
const series = ref([
  {
    name: props.dataProps.label,
    data: dates.value, // Lưu ý: Bạn cần định nghĩa biến dates trước đó
  },
]);

const chartOptions = ref({
  chart: {
    type: "area",
    stacked: false,
    height: 350,
    zoom: {
      type: "x",
      enabled: true,
      autoScaleYaxis: true,
    },
    toolbar: {
      autoSelected: "zoom",
    },
  },
  dataLabels: {
    enabled: false,
  },
  markers: {
    size: 0,
  },
  fill: {
    type: "gradient",
    gradient: {
      shadeIntensity: 1,
      inverseColors: false,
      opacityFrom: 0.5,
      opacityTo: 0,
      stops: [0, 90, 100],
    },
  },
  yaxis: {
    max: props.dataProps.maxYCharts,
    labels: {
      formatter: function (val) {
        return val.toFixed(0);
      },
    },
    title: {
      text: props.dataProps.label,
    },
  },
  xaxis: {
    type: "datetime",
  },
  tooltip: {
    shared: false,
    y: {
      formatter: function (val) {
        return val.toFixed(0);
      },
    },
  },
});
</script>
