<template>
  <div class="container-light-bulb">
    <label for="lightSwitch"
      ><b>{{ label }}</b></label
    >
    <div
      class="light-container"
      :style="{
        backgroundColor: isLightOn ? 'yellow' : '#d0d0dd',
        boxShadow: isLightOn ? '0 0 10px yellow, 0 0 20px yellow' : 'none',
      }"
    ></div>
    <button @click="toggleLight" class="button-light-blue">
      {{ isLightOn ? "Tắt" : "Bật" }}
    </button>
  </div>
</template>

<script setup>
const emit = defineEmits();
// props
const props = defineProps({
  statusLight: Object,
});

const isLightOn = ref(props.statusLight.isLightOn);
const label = ref(props.statusLight.label);

const toggleLight = () => {
  isLightOn.value = !isLightOn.value;
};

//
watch(
  () => isLightOn.value,
  () => {
    console.log("isLightOn.value", isLightOn.value);
    const dataEmit = isLightOn.value ? 1 : 0;
    emit("ledOne", dataEmit);
  }
);
</script>

<style lang="scss" scoped>
.container-light-bulb {
  border-radius: 10px;
  background-color: #d1ebef;
  width: 150px;
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0px 20px;
  .light-container {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 10px 0;
    transition: background-color 0.3s, box-shadow 0.3s;
  }
  .button-light-blue {
    border: 1px solid #8282b5;
    border-radius: 5px;
    background-color: #8282b5;
    color: white;
    padding: 3px 20px;
    margin: 10px;
  }
}
</style>
