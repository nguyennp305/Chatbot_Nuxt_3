<template>
  <div id="humidity-chart">
    <apexchart
      type="radialBar"
      height="250"
      :options="chartOptions"
      :series="series"
    ></apexchart>
  </div>
</template>

<script setup>
import { ref } from "vue";

// variable
const props = defineProps({
  dataProps: Object,
});
const series = ref([props.dataProps.series]); // Change the humidity value here

const chartOptions = {
  chart: {
    height: 250,
    type: "radialBar",
  },
  plotOptions: {
    radialBar: {
      startAngle: -135,
      endAngle: 135,
      hollow: {
        margin: 0,
        size: "70%",
      },
      track: {
        background: "#333",
        strokeWidth: "67%",
      },
      dataLabels: {
        name: {
          offsetY: -10,
          show: true,
          color: "#888",
          fontSize: "17px",
        },
        value: {
          color: "#111",
          fontSize: "26px",
          show: true,
          formatter: function (val) {
            return val + props.dataProps.measure;
          },
        },
      },
    },
  },
  fill: {
    type: "gradient",
    gradient: {
      shade: "dark",
      type: "horizontal",
      gradientToColors: ["#87D4F9"],
      stops: [0, 100],
    },
  },
  stroke: {
    lineCap: "round",
  },
  labels: [props.dataProps.label],
};
</script>
