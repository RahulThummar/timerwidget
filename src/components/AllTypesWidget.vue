<template>
  <div class="main-box ms-3">
    <div class="top-menu">
      <!-- <img
        :src="UnfilledBoxes"
        alt="UnfilledBoxes"
        :style="{ width: '32px', height: '32px' }"
      /> -->
      <div>
        <img class="pointer first" :src="selectTimer ? FilledTimer : UnfilledTimer" alt="Group244"
          :style="{ width: '32px', height: '32px' }" @click="handleChangeWidgetType('selectTimer')" />
          <!-- <span class="a"></span> -->
      </div>
      <div class="b">
        <img class="pointer" :src="selectSpinner ? FilledSpinner : UnfilledSpinner" alt="Group244"
          :style="{ width: '32px', height: '32px' }" @click="handleChangeWidgetType('selectSpinner')" />
      </div>
      <div class="c">
        <img class="pointer" :src="selectTrafficLight ? FilledTrafficLight : UnfilledTrafficLight" alt="Group244"
          :style="{ width: '32px', height: '32px' }" @click="handleChangeWidgetType('selectTrafficLight')" />

      </div>
    </div>

    <!-- :class="{
        'd-none': !selectTimer,
        '': selectTimer,
      }" -->
    <div :style="{
      zIndex: !selectTimer ? '-1' : '1',
      opacity: !selectTimer ? '0' : '1',
    }">
      <TimerWidget />
    </div>

    <div :class="{
      'd-none': !selectSpinner,
      '': selectSpinner,
    }">
      <SpinnerWidget />
    </div>
    <div :class="{
      'd-none': !selectTrafficLight,
      '': selectTrafficLight,
    }">
      <TrafficLightWidget />
    </div>
  </div>
</template>

<script>
// import UnfilledBoxes from "@/assets/images/AllTypesWidget/UnfilledBoxes.png";
import UnfilledTimer from "@/assets/images/AllTypesWidget/UnfilledTimer.png";
import UnfilledSpinner from "@/assets/images/AllTypesWidget/UnfilledSpinner.png";
import UnfilledTrafficLight from "@/assets/images/AllTypesWidget/UnfilledTrafficLight.png";

import FilledSpinner from "@/assets/images/AllTypesWidget/FilledSpinner.png";
import FilledTimer from "@/assets/images/AllTypesWidget/FilledTimer.png";
import FilledTrafficLight from "@/assets/images/AllTypesWidget/FilledTrafficLight.png";

import TimerWidget from "./TimerWidget.vue";
import TrafficLightWidget from "./TrafficLightWidget.vue";
import SpinnerWidget from "./SpinnerWidget.vue";

export default {
  components: {
    TimerWidget,
    TrafficLightWidget,
    SpinnerWidget,
  },
  data() {
    return {
      // UnfilledBoxes: UnfilledBoxes,
      UnfilledTimer: UnfilledTimer,
      UnfilledSpinner: UnfilledSpinner,
      UnfilledTrafficLight: UnfilledTrafficLight,

      FilledTimer: FilledTimer,
      FilledSpinner: FilledSpinner,
      FilledTrafficLight: FilledTrafficLight,

      selectTimer: true,
      selectSpinner: false,
      selectTrafficLight: false,

      prevSelected: "selectTimer",
    };
  },
  methods: {
    handleChangeWidgetType(type) {
      this[this.prevSelected] = false;
      this[type] = true;
      this.prevSelected = type;
    },
  },
};
</script>

<style scoped>
@import "../assets/style.css";
</style>
