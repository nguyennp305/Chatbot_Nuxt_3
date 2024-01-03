<template>
  <div class="living-rom">
    <div class="light-bulb">
      <IotChallangeLightBulb :status-light="lightBulb1" @led-one="ledOne" />
      <IotChallangeLightBulb :status-light="lightBulb2" @led-one="ledTwo" />
      <IotChallangeLightBulb :status-light="lightBulb3" @led-one="ledThree" />
    </div>
    <div class="charts">
      <div>
        <IotChallangeRadialBar :data-props="temperature" />
        <IotChallangeLineCharts :data-props="temperature" />
      </div>
      <div>
        <IotChallangeRadialBar :data-props="humidity" />
        <IotChallangeLineCharts :data-props="humidity" />
      </div>
    </div>
  </div>
</template>

<script setup>
// data
const getData = ref({
  topic: "room_1",
  temp: 25.6,
  hum: 66.4,
  status_led1: 0,
  status_led2: 0,
  status_led3: 1,
});

// props for light bulb_1
const lightBulb1 = ref({
  isLightOn: getData.value.status_led1 === 1 ? true : false,
  label: "Bóng đèn 1",
});

// props for light bulb_2
const lightBulb2 = ref({
  isLightOn: getData.value.status_led2 === 1 ? true : false,
  label: "Bóng đèn 2",
});

// props for light bulb_3
const lightBulb3 = ref({
  isLightOn: getData.value.status_led3 === 1 ? true : false,
  label: "Bóng đèn 3",
});

// props for temperature
const currentDate = new Date();
const gmtOffset = 7 * 60 * 60 * 1000; // 7 giờ * 60 phút * 60 giây * 1000 milliseconds
// Timestamp cho ngày hiện tại
const currentTimestamp = currentDate.getTime() + gmtOffset;
console.log("newdate", currentTimestamp);
console.log("currentDate", new Date(currentTimestamp));

// Timestamp cho ngày hiện tại kết hợp với một giờ trước
const oneHourAgoTimestamp = currentDate.getTime() - 3600000 + gmtOffset; // 1 giờ = 3600 giây * 1000
// Timestamp cho ngày hiện tại kết hợp với hai giờ trước
const twoHoursAgoTimestamp = currentDate.getTime() - 7200000 + gmtOffset; // 2 giờ = 7200 giây * 1000

const temperature = ref({
  series: getData.value.temp,
  label: "Temperature",
  measure: " ℃",
  dates: [
    [twoHoursAgoTimestamp, 24], // timestamp, value
    [oneHourAgoTimestamp, 20],
    [currentTimestamp, 34],
  ],
  maxYCharts: 50,
});

// props for humidity
const humidity = ref({
  series: getData.value.hum,
  label: "Humidity",
  measure: " %H",
  dates: [
    [1609459200000, 90], // timestamp, value
    [1609545600000, 40],
    [1609632000000, 74],
  ],
  maxYCharts: 100,
});

// watch getData
watch(
  () => getData.value,
  () => {
    console.log("getData.value", getData.value);
  },
  { deep: true }
);

// function
const ledOne = (data) => {
  console.log("data-ledOne", data);
};
const ledTwo = (data) => {
  console.log("data-ledTwo", data);
};
const ledThree = (data) => {
  console.log("data-ledThree", data);
};
</script>

<style lang="scss" scoped>
.living-rom {
  .light-bulb {
    display: flex;
    border-radius: 20px;
    box-sizing: border-box;
    background: #ecf0f3;
    box-shadow: 14px 14px 20px #cbced1, -14px -14px 20px white;
    justify-content: space-around;
    padding: 20px;
    margin-bottom: 20px;
  }
  .charts {
    display: flex;
    justify-content: space-around;
    border-radius: 20px;
    box-sizing: border-box;
    background: #ecf0f3;
    box-shadow: 14px 14px 20px #cbced1, -14px -14px 20px white;
    padding: 20px;
  }
}
</style>
